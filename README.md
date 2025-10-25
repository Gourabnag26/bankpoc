import React, { useState, useEffect } from "react";
import SprintButtons from "./components/SprintButtons";
import MembersList from "./components/MembersList";
import WorkItemsTable from "./components/WorkItemsTable";
import SummaryCards from "./components/SummaryCards";
import SprintModal from "./components/SprintModal";
import MemberModal from "./components/MemberModal";
import PieChartOverview from "./components/PieChartOverview";

import {
  fetchSprints,
  createSprint,
  fetchMembers,
  addMember,
  fetchWorkItems,
} from "../api";

export default function Dashboard() {
  const [sprints, setSprints] = useState([]);
  const [activeSprintId, setActiveSprintId] = useState(null);
  const [members, setMembers] = useState([]);
  const [workItems, setWorkItems] = useState([]);

  // Modal state
  const [showSprintModal, setShowSprintModal] = useState(false);
  const [showMemberModal, setShowMemberModal] = useState(false);

  // Form state
  const [newSprintName, setNewSprintName] = useState("");
  const [memberName, setMemberName] = useState("");
  const [role, setRole] = useState("Lead");
  const [team, setTeam] = useState("Dev");

  // Load sprints on mount
  useEffect(() => {
    loadSprints();
  }, []);

  // Load members and work items when active sprint changes
  useEffect(() => {
    if (!activeSprintId) return;
    loadMembers(activeSprintId);
    loadWorkItems(activeSprintId);
  }, [activeSprintId]);

  async function loadSprints() {
    const data = await fetchSprints();
    setSprints(data);
    if (data.length) setActiveSprintId(data[0].id);
  }

  async function loadMembers(sprintId) {
    const data = await fetchMembers(sprintId);
    setMembers(data);
  }

  async function loadWorkItems(sprintId) {
    const data = await fetchWorkItems(sprintId);
    setWorkItems(data);
  }

  async function handleCreateSprint() {
    if (!newSprintName) return;
    const sprint = await createSprint({ name: newSprintName });
    setSprints([sprint, ...sprints]);
    setNewSprintName("");
    setActiveSprintId(sprint.id);
    setShowSprintModal(false);
  }

  async function handleAddMember(e) {
    e.preventDefault();
    if (!memberName) return;
    await addMember(activeSprintId, { memberName, role, team });
    setMemberName("");
    setRole("Lead");
    setTeam("Dev");
    setShowMemberModal(false);
    loadMembers(activeSprintId);
  }

  return (
    <div className="min-h-screen p-6 bg-gray-900 text-white">
      <header className="flex justify-between mb-6">
        <h1 className="text-3xl font-bold">Sprint Dashboard</h1>
        <div className="flex gap-2">
          <button
            onClick={() => setShowSprintModal(true)}
            className="px-4 py-2 bg-indigo-600 rounded"
          >
            + Add Sprint
          </button>
          {activeSprintId && (
            <button
              onClick={() => setShowMemberModal(true)}
              className="px-4 py-2 bg-green-600 rounded"
            >
              + Add Member
            </button>
          )}
        </div>
      </header>

      {/* Sprint Buttons */}
      <SprintButtons
        sprints={sprints}
        activeSprintId={activeSprintId}
        onSelectSprint={setActiveSprintId}
      />

      {/* Summary Cards */}
      <SummaryCards
        sprintsCount={sprints.length}
        totalTasks={workItems.length}
        completedTasks={workItems.filter((i) => i.status === "Done").length}
        pendingTasks={workItems.filter((i) => i.status !== "Done").length}
      />

      {/* Pie Chart */}
      <PieChartOverview workItems={workItems} />

      {/* Members List */}
      <MembersList members={members} />

      {/* Work Items Table */}
      <WorkItemsTable workItems={workItems} />

      {/* Modals */}
      <SprintModal
        show={showSprintModal}
        onClose={() => setShowSprintModal(false)}
        sprintName={newSprintName}
        setSprintName={setNewSprintName}
        onCreate={handleCreateSprint}
      />
      <MemberModal
        show={showMemberModal}
        onClose={() => setShowMemberModal(false)}
        memberName={memberName}
        setMemberName={setMemberName}
        role={role}
        setRole={setRole}
        team={team}
        setTeam={setTeam}
        onAdd={handleAddMember}
      />
    </div>
  );
}
