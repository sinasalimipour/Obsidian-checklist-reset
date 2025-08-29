


- [ ] **1**
- [ ] **2**
- [ ] **3** 
- [ ] **Finish**

```button
name Reset Checklist
type command
action Manual Reset Checklist
```



```dataviewjs

// Helper: reset function
async function resetChecklist() {
    const file = dv.current().file.path;
    const af = app.vault.getAbstractFileByPath(file);
    const content = await app.vault.read(af);
    const reset = content.replace(/\[x\]/gi, "[ ]");
    await app.vault.modify(af, reset);
    new Notice("Checklist has been reset ✅");
}

// 1️⃣ Manual Command
app.commands.addCommand({
    id: "reset-checklist",
    name: "Manual Reset Checklist",
    callback: resetChecklist
});

// 2️⃣ Auto Reset (check every 5s)
setInterval(async () => {
    const file = dv.current().file.path;
    const af = app.vault.getAbstractFileByPath(file);
    const content = await app.vault.read(af);

    // If there are no unchecked boxes left → reset
    if (!content.match(/- \[ \]/)) {
        await resetChecklist();
    }
}, 5000);


```

