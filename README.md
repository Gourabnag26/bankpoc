import React from "react"
import { Input, TextField } from "@ucl/ui-components"


interface ItimeInputProps{
    time:string,
    setTime:(value:string)=>void,
    
}

const TimeInput=({time,setTime}:ItimeInputProps)=>{
    const handleBlur=()=>{
        if(!time)
        return;
       const [hours="",minutes=""]=time.split(":");
       const formatedHours=hours.padStart(2,"0");
       const formatedMinutes=minutes.padStart(2,"0");
       setTime(`${formatedHours}:${formatedMinutes || "00"}`)

    }
    const handleTimeChange=(e:any)=>{
        let value=e.target.value;
        value=value.replace(/[^0-9:]/g,"");
        if(value.length==2 && ! value.includes(":"))
        {
            value=value+":"
        }
        const [hours,minutes]=value.split(":")
        if(hours && Number(hours) >23)
        return;
        if(minutes && Number(minutes) >59 )
        return;
        setTime(value)
    }
    return(
        <Input titleLabel="" sx={{width:"70px",height:"40px !important",textAlign:"center"}} value={time} characterLimit={4} onChange={(e:any)=>{handleTimeChange(e)}} onBlur={handleBlur}/>
    )

}

export default TimeInput;
