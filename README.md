import React from "react";
import { Box,Checkbox,Typography } from "@ucl/ui-components";
interface ICheckboxOption{
    id:string;
    label:string;
    value:string;
}

interface ICardCheckboxProps{
    title:string;
    checkboxes: ICheckboxOption[];
    selectedValues : string[];
    onChange: (newSelectedValues : string[])=> void;
}

const CardCheckbox=({title,checkboxes,selectedValues,onChange}:ICardCheckboxProps)=>{
    console.log("selected",selectedValues);
   const handleCheckboxChange=(id:string)=>{
    const updated=selectedValues.includes(id)?selectedValues.filter((val)=>val!==id):[...selectedValues,id];
    onChange(updated)
   }
   return(
    <Box sx={{background:"#F8F8F8",padding:"10px",width:"250px",height:"200px", fontSize:"12px", border:"1px dotted black"}}>
     <Typography variant="body1" >
     {title}
     </Typography>
     <Box sx={{padding:'10px'}}>
        {
            checkboxes.map((checkbox:ICheckboxOption)=>(
                <Checkbox label={checkbox.label} checked={selectedValues.includes(checkbox.id)} onChange={()=>{
                    handleCheckboxChange(checkbox.id)
                }}/>
            ))
        }
     </Box>
    </Box>
   )
}

export default CardCheckbox
