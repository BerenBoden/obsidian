<%* 
const sourceFilePath = "path/to/folder/list.md";
const sourceFile = app.vault.getAbstractFileByPath(sourceFilePath);
const sourceFileContent = await app.vault.read(sourceFile);
const destFolder = app.vault.getAbstractFileByPath("path/to/folder")
let lines = sourceFileContent.split("\n");
for (let line of lines){
        if(line.trim() !== ""){
                await tp.file.create_new("", line.trim() + ".md", false, destFolder);
        }
}
%>