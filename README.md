# photo_system
This project is for compiling metadata from and organizing downloaded iPhone photos.

The final image file names will be [subject]\_[date]\_[record-number].jpeg
- All images will be in the .jpeg file format rather than .png for file size optimization and EXIF metadata compatibility. 
- The [date] reflects the date the picture was taken, and it will follow the YYYY-MM-DD format. The [record-number] differentiates photos from each date, and counts up from 0001 to 9999. These are automatically generated using image metadata and existing records from the index of images. 
- The [subject] component will either be a shorthanded place name for scenery and people pictures, a shorthanded lowest-level taxon name for bug pictures, or a shorthanded restaurant name for food pictures. This will be manually added later.
- Each of these will need a corresponding table with the shorthand, the proper name, and the associated information for each (identification taxonomy for bugs and restaurant taxonomy for restaurants, and place name and places taxonomy for all three).
- I will build a master index for processed images with the following columns: date (string, YYYY-MM-DD), record_number (int, 0001-9999), place_name_short (string, flexible-place-name), subject_type (string, scenery/bugs/people/food), subject_name_short (string, taxon-or-restaurant-name; None for scenery/people).
- Shorthanded place, taxon, and restaurant names will have a corresponding entry in the taxonomy databases of each.

The workflow will be as follows:
1. Get downloaded photos into 1_downloaded_photos/.
2. Run a script to convert files to .jpeg format and put them in 2_converted_photos/.
3. Run a metadata-gathering script to capture the following intrinsic attributes into a temporary preliminary index in photo_data/: a) the initial file name, b) the date, c) the location, and d) the camera maker (reflects iphone, samsung, camera, or screenshot source). 
    - Files that do not have a date will be moved from 2_converted_photos/ to 2b_fix_no_date/. Manually add dates, move the photos back to 2_converted_photos/, and re-run the script.
4. Run a script that checks the master index for the next-available [date]_[number] file name combinations, assigns the appropriate combination to each entry in the preliminary index, adds the assigned combinations to the master index, and renames the files accordingly into 3_renamed_photos_missing_subject/. This script will verify that the master index exists in photo_data/, creating it otherwise.
5. Use a tkinter application to select photos and bulk-add location, image type, and subject attributes to their respective entries in the master index (filling in the columns place_name_short, subject_type, and subject_name_short, respectively).
6. Run a final script to add subject information to the names of the files in 3_renamed_photos_missing_subject/, placing the final photos for further organization and website use in 4_fully_renamed_photos/. This script will then delete the preliminary index in photo_data/.