---
title: Git 저장소 empty 폴더 관리
date: 2020-03-03 14:07:47
tags: [git, gitlab, gitkeep]
---
## 개요
git을 사용하다보면, 폴더만 생성되었거나 폴더 하위의 파일이 없는 경우 stage에 올릴 수 없다.(commit, push 불 가능)  
때문에 .gitkeep 파일을 생성하여 해당 경로를 공유한다.  
만약, 폴더 내 파일이 생성되는 경우 .gitkeep의 불필요한 파일은 삭제해 주는 것이 좋다.  

## 작업
- Main.java
```
package fileMaker;
 
import java.io.IOException;
 
public class Main {
     
    public static void main(String[] args) {
         
        if(args.length == 0) {
            System.out.println("경로를 입력해 주세요.");
        }else {
            System.out.println("##########################################");
            System.out.println("######## Directory Keeper Starts!!! ######");
            System.out.println("##########################################");
             
            System.out.println("INPUT DIR : "+ args[0]);
             
            FileManager fileManager = new FileManager();
            try {
                fileManager.filePathTravel(args[0]);
            }catch(IOException ie) {
                System.out.println("파일 I/O 문제 발생!!");
                ie.printStackTrace();
            }catch(Exception e) {
                System.out.println("Exception 발생 !!");
                e.printStackTrace();
            }
        }
         
    }
}
```
- FileManager.java
```
package fileMaker;
 
import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
 
public class FileManager {
     
     
    public void filePathTravel(String source) throws IOException {
        File dir = new File(source);
         
        File[] fileList = dir.listFiles();
         
        if(fileList.length == 0) {
            makeGitKeep(source);
        }else {
            removeKeepFiles(source);
             
            for(int i=0; i<fileList.length; i++) {
                File file = fileList[i];
                 
                if(file.isDirectory()) {
                    filePathTravel(file.getCanonicalPath().toString());
                }
            }
        }
    }
     
    /**
     * @param directory
     * @description 전달받은 경로에 .gitkeep 파일 생성.
     * @return 파일 생성 여부.
     */
    public boolean makeGitKeep(String directory) throws IOException {
        String fileName = ".gitkeep";
         
        File f = new File(directory + File.separator + fileName);
         
        return f.createNewFile();
    }
     
    public boolean removeKeepFiles(String directory) throws IOException{
        File dir = new File(directory);
        File[] fileList = dir.listFiles();
        ArrayList<String> removeFileNames = new ArrayList<String>();
        // 불필요한 파일 목록
        removeFileNames.add(".gitkeep");
        removeFileNames.add("deleteme.txt");
         
        int fileCount = 0;
        int folderCount = 0;
         
        for(int i=0; i<fileList.length; i++) {
            File tmp = fileList[i];
             
            if( tmp.isFile() ) {
                if( !removeFileNames.get(0).equalsIgnoreCase(tmp.getName()) && !removeFileNames.get(1).equalsIgnoreCase(tmp.getName()) ) {
                    fileCount++;
                }
            }
            if( tmp.isDirectory() ) {
                folderCount++;
            }
        }
         
        if( (fileCount > 0) || (fileCount == 0 && folderCount > 0 ) ) {
            File gitKeepFile = new File(directory + File.separator + removeFileNames.get(0));
            File deletemeFile = new File(directory + File.separator + removeFileNames.get(1));
             
            if(gitKeepFile.exists()) {
                if(!gitKeepFile.delete()) {
                    return false;
                }
            }
            if(deletemeFile.exists()) {
                if(!deletemeFile.delete()) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

## 사용법
executable jar로 export  
필자의 경우 EmptyFolder.jar로 파일명을 정했다.  
```
java EmptyFolder.jar {repository_path}
```
비어있는 폴더의 경우 .gitkeep 파일을 생성한다.  
비어있지 않은 폴더에서 .gitkeep이나 deleteme.txt 등 불필요한 파일이 존재하면 삭제한다.  

## 사양
Java : javac 1.8.0_201-1-ojdkbuild
openjdk 1.8 버전에서 동작하는 코드이다.