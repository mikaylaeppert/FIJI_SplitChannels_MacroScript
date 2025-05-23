# FIJI_SplitChannels_MacroScript
This ImageJ macro processes all images in a user-selected folder by opening each image splitting channels if multi-channel, and saving each channel as a separate TIFF in a new output subfolder.

This FIJI (ImageJ) macro script automates the batch processing of multi-channel images in a selected directory. It opens each image, splits the channels if needed, and saves each channel as a separate TIFF file in an organized output subfolder.

📁 What It Does

Processes all image files in a user-selected directory.
Detects and splits multi-channel images using ImageJ's Split Channels function.
Saves each individual channel as a separate TIFF file in a new output/ subfolder.

🛠 Requirements

FIJI (ImageJ distribution)
Input files must be readable by FIJI (e.g., TIFF, LSM, etc.)
Script must be run from within FIJI (via Plugins > Macros > Run...)

▶️ How to Use

Open FIJI.
Go to Plugins > Macros > Run...
Select the SplitChannels_inDirectory.ijm script.
Choose the input folder when prompted.
The script will:
Create an output/ folder inside the selected directory.
Save each channel of every multi-channel image as a separate TIFF.

📝 Notes

The script skips subfolders — it only processes image files directly inside the selected folder.
Already single-channel images are skipped from the splitting step but still saved.
Output files retain the original channel titles after splitting (e.g., imgName (c1).tif, imgName (c2).tif).

📂 Output Example

    /your/input/folder/
    ├── image1.tif
    ├── image2.tif
    └── output/
        ├── image1 (c1).tif
        ├── image1 (c2).tif
        ├── image2 (c1).tif
        └── image2 (c2).tif

📄 License

Feel free to modify or adapt this script for your needs.


    dir = getDirectory("Select A folder");  // Prompt user to select a folder and store its path in 'dir'

    fileList = getFileList(dir);  // Get a list of all files and folders in the selected directory

    output_dir = dir + File.separator + "output" + File.separator;  // Define output directory path as a subfolder named "output" inside 'dir'

    File.makeDirectory(output_dir);  // Create the output directory if it doesn't already exist

    setBatchMode(true);  // Enable batch mode to speed up processing and suppress image display updates

    for (i = 0; i < lengthOf(fileList); i++) {  // Loop through every item in the file list

        current_imagePath = dir + fileList[i];  // Construct the full path to the current file

        if (!File.isDirectory(current_imagePath)) {  // Check if the current item is NOT a directory (i.e., is a file)
        
            open(current_imagePath);  // Open the current image file
        
            getDimensions(width, height, channels, slices, frames);  // Get image dimensions and number of channels
        
            if (channels > 1) run("Split Channels");  // If image has multiple channels, split them into separate images
        
            ch_nbr = nImages;  // Set 'ch_nbr' to the number of currently open images (channels after split)
        
            for (c = 1; c <= ch_nbr; c++) {  // Loop over each channel image
            
                selectImage(c);  // Select the c-th channel image
            
                currentImage_name = getTitle();  // Get the title (filename) of the current image
            
                saveAs("tiff", output_dir + currentImage_name);  // Save the image as TIFF in the output directory
            }
        
            run("Close All");  // Close all open images before processing the next file
        }
    }

    setBatchMode(false);  // Disable batch mode to return to normal ImageJ behavior
