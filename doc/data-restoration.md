# Data Restoration

## Pre Requisites:
- Ready with Expected Data (ED) backup-folder.

## STEP - I] Replace `data` folder
- Stop running `gstudio` server
    - `docker stop gstudio`
  ![1](https://user-images.githubusercontent.com/21193492/50424177-5965e280-0886-11e9-8afd-01397845b69f.png)
 

- [Check]: If server is stopped or not.
    + `docker ps`
  


    + Above command should not show any entry of container - `gstudio`  
- From terminal, move to expected data path
    - `cd </path/where/data/is/mounted>`
  
  ![snapshot](/home/sheetal/Pictures/snapss/2a.png)

- Rename existing data folder with existing school *server-id*.
    + e.g: `data` --> `data-mz1`
  
  ![snapshot](/home/sheetal/Pictures/snapss/3.png)

    + This step is optional to make provision of new backup-folder containing `mz-9` users which will be renamed as `data`
    + *Note: This step will persist `N` nos of folders after period of time, check for your HDD space*
- Copy/Move ED folder here. Choose either of following
    + **Copy** (notice **`.`** at end): `rsync -avzPh <path/to/ED/backup-folder> .`
    + **Move** (notice **`.`** at end): `mv -v <path/to/ED/backup-folder> .`
  
  ![snapshot](/home/sheetal/Pictures/snapss/4.png)    
  
  **note**: Here Expected Data Backup-folder named as: `schooldata` 

- Rename copied/moved ED backup-folder to `data`:
    + **Rename**: `mv  -v  <name of ED backup-folder>  data`
  
  ![snapshot](/home/sheetal/Pictures/snapss/5.png)

- Start the  `gstudio` server:
    + `docker start gstudio`
  
  ![snapshot](/home/sheetal/Pictures/snapss/6.png)

- Navigate to  `clixserver.tiss.edu` to check whether we can login by `mz-9`:
    + Since 'mz-9' users have not been restored into postgres database, the result in below snapshot displays the login by the previous user i.e.`mz-1`
  
  ![snapshot](/home/sheetal/Pictures/snapss/7.png)
    
    + To display `mz-9` school information, we can check in `Workspaces` of `clixserver.tiss.edu` :
  
  ![snapshot](/home/sheetal/Pictures/snapss/8.png)   
    
    + To show the comments of the `mz-9` school server, it displays the error as specified below:
  
  ![snapshot](/home/sheetal/Pictures/snapss/9.png)

  - **note** : It displays error since, mz-9 users are not restored into the postgres database which is explained in the further step.

## STEP - II] Import users/`sql` data
- The step II describes importing `mz-9` users into the postgres database and removing the previous `mz-1` users from postgres.
    + To remove the previous postgres database which consist of `mz-1` users, follow the steps in the given snapshot: 
  
  ![snapshot](/home/sheetal/Pictures/snapss/10.png)      
  
  **note**: to come out of the postgres, press `Ctrl+d` twice.
    
    + Next step is to import the `mz-9` users into postgres. To do so, we need to find the latest postgres dump sql file named as **"pg_dump_all.sql"** which is found at following path: `/data/postgres-dump/pg_dump_all.sql`
  
  ![snapshot](/home/sheetal/Pictures/snapss/11.png)

    + Restore the `mz-9` users into postgres database by following command:
  
  ![snapshot](/home/sheetal/Pictures/snapss/13.png)

    + Result of Restoration: The following snapshot display that the `mz-9` users has been successfully restored:
  
  ![snapshot](/home/sheetal/Pictures/snapss/14.png)

    + To view the comments of the `mz-9` school server:
  
  ![snapshot](/home/sheetal/Pictures/snapss/15.png)
  
  **note**: Earlier it had displayed the error since `mz-9` users were not restored at step I.


## STEP - III] Replace settings files
- To login with the `mz-9` user, we need to replace the configuration file i.e. `settings` file ( which includes both local_settings.py and server_settings.py):
  
  ![snapshot](/home/sheetal/Pictures/snapss/16.png)


## Post Restoration Checkpoints:
1. ..
2. ..
3. ..

---

## Summary:
- 2 data steps(step - I & II) and 1 configuration/settings step(step - III)
