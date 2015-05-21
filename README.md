# 4
import java.awt.List;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.ArrayList;
public class Filesystem {
	 public static ArrayList<String> result = new ArrayList<String>();
    	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		createFolder(new File("D:\\Filesystem"));
		createFile(new File("D:/Filesystem/f.txt"));
		writetoFile(new File("D:/Filesystem/f.txt"));
		readFile(new File("D:/Filesystem/file.txt"));
		listoffilesinFolder(new File("D:/Filesystem"));
		searchDirectory(new File("C:/Users/public/Documents"),"haar.txt");
		int count = result.size();
	    if (count !=0){
	    System.out.println(result.size());
	    System.out.println(result);
	    }
	    //copying 2 files
	    File source = new File("D:/Filesystem/f.txt");
        File dest = new File("D:/Filesystem/f1.txt");
        if (!dest.exists()){
        	createFile(new File("D:/Filesystem/f1.txt"));
        }

	    copyFile(source, dest);
        //copying to folders
		File srcFolder = new File("D:\\Filesystem");
    	File destFolder = new File("D:\\Filesystem-new");
    	//make sure source exists
    	if(!srcFolder.exists()){
           System.out.println("Directory does not exist.");
           System.exit(0);
        }else{
           try{
        	copyFolder(srcFolder,destFolder);
           }catch(IOException e){
        	e.printStackTrace();
        	//error, just exit
                System.exit(0);
           }
        }
    	System.out.println("Done");
	}
//list of files in a given folder
private static void listoffilesinFolder(File file){
		// TODO Auto-generated method stub
	File[] list = file.listFiles();
	if (file.listFiles() != null){
	 for (File file1:list){
		if (file1.isDirectory()){
		System.out.println(file1.getName());
		listoffilesinFolder(file1);
		}else{
		if (file1.isFile()){
			System.out.println(file1.getName());
		  }
		}
	 }	
	}
}    
	//read a file
private static void readFile(File file) {
		// TODO Auto-generated method stub
	try {
		String sCurrentLine;
		BufferedReader br = new BufferedReader(new FileReader(file));
		while( (sCurrentLine=br.readLine()) != null) {
			System.out.println(sCurrentLine);
		}
		br.close();
	} catch (IOException e) {
		e.printStackTrace();
	} 
}
//write to a file
private static void writetoFile(File file) {
		// TODO Auto-generated method stub
	try{
		String data = "HELLO WORLD";
	 //if file doesnt exists, then create it
		if(!file.exists()){
			file.createNewFile();
		}
		FileWriter fileWritter = new FileWriter(file.getAbsoluteFile());
		System.out.println(file.getAbsolutePath());
	        BufferedWriter bufferWritter = new BufferedWriter(fileWritter);
	        bufferWritter.write(data);
	        bufferWritter.close();
        System.out.println("Done");
	}catch(IOException e){
		e.printStackTrace();
	}
}
//create a file 
private static void createFile(File file) {
		// TODO Auto-generated method stub
	try{	 
	     if (!file.exists()){
	    	  file.createNewFile();
	        System.out.println("File is created!");
	      }else{
	        System.out.println("File already exists.");
	      }
	}
	catch(IOException e){
		e.printStackTrace();
	}
}
//create a folder
private static void createFolder(File file) {
		// TODO Auto-generated method stub
	if (!file.exists()) {
		if (file.mkdir()) {
			System.out.println("Folder Filesystem  is created!");
		} else {
			System.out.println("Failed to create directory!");
		}
 }
}		
//search a file
public static void searchDirectory(File directory, String fileNameToSearch) {
	System.out.println(directory);
	if (directory.isDirectory()) {
		search(directory,fileNameToSearch);
	} else {
	    System.out.println(directory.getAbsoluteFile() + " is not a directory!");
	}
 
  }
 public static void search(File file, String fileNameToSearch) {
	if (file.isDirectory()) {
	  System.out.println("Searching directory ... " + file.getAbsoluteFile());
 
            //do you have permission to read this directory?	
	  if (file.canRead()) {
	    if (file.listFiles() != null){
		  for (File temp : file.listFiles()) {
		    if (temp.isDirectory()) {
		    	System.out.println("folder"+temp.getName());
			search(temp,fileNameToSearch);
		    } else{
		    	if (temp.isFile()){
	     			if ((temp.getName()).equals(fileNameToSearch)) {			
		        	    result.add(temp.getAbsoluteFile().toString());
			        }
		    	}
		      }
	      }
	    }
	 } else {
		System.out.println(file.getAbsoluteFile() + "Permission Denied");
	 }
   }
}
 //copy folder to folder
 public static void copyFolder(File src, File dest)
	    	throws IOException{
	 	    	if(src.isDirectory()){
	 	    		//if directory not exists, create it
	    		if(!dest.exists()){
	    		   dest.mkdir();
	    		   System.out.println("Directory copied from " 
	                              + src + "  to " + dest);
	    		}
	 	    		//list all the directory contents
	    		String files[] = src.list();
	 	    		for (String file : files) {
	    		   //construct the src and dest file structure
	    		   File srcFile = new File(src, file);
	    		   File destFile = new File(dest, file);
	    		   //recursive copy
	    		   copyFolder(srcFile,destFile);
	    		}
	 	    	}else{
	    		//if file, then copy it
	    		//Use bytes stream to support all file types
	    		InputStream in = new FileInputStream(src);
	    	        OutputStream out = new FileOutputStream(dest); 
	 	    	        byte[] buffer = new byte[1024];
	 	    	        int length;
	    	        //copy the file content in bytes 
	    	        while ((length = in.read(buffer)) > 0){
	    	    	   out.write(buffer, 0, length);
	    	        }
	 	    	        in.close();
	    	        out.close();
	    	        System.out.println("File copied from " + src + " to " + dest);
	    	}
	    }

private static void copyFile(File source, File dest)
        throws IOException {
    InputStream input = null;
    OutputStream output = null;
    try {
        input = new FileInputStream(source);
        output = new FileOutputStream(dest);
        byte[] buf = new byte[1024];
        int bytesRead;
        while ((bytesRead = input.read(buf)) > 0) {
            output.write(buf, 0, bytesRead);
        }
   } finally {
        input.close();
        output.close();
    }
}
}

	 
	
	


	 

	 



	

