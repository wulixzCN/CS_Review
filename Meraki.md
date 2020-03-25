```

 
    
```

#Introduce Office Face Recognition 

System Structure
![Image text](https://wwwin-github.cisco.com/zhenxu2/Merak-Camera-Campus-iIdentity-Recognition/blob/master/media/calculate.png)



## the process of this system






> preparation of the program
>  write the config file	
 1. use the 'InfoRegister.py' to register the staff of the whole database
    accurate parameters : 
     1. the path of staff's photo/profile
     2. the name of the staff
     3. the id of the company
     4. the sex of the staff 
      first : create a class of face_feature_register()
      second : get_face_feature_and_insert(photo=photoname,name= name, employeeid=employeeid,sex=sex) 



## Function of each python script

- PersonInfo.py
    class of Person (initialize name,employee_id,register_time=None,sex=None,face_feature=None)

- loadDatabase.py

    class of Database:
        if there is no table, it will create a table automatically
        
    function :
        select encoding of all staff (no parameter)
        select encoding of specified staff (needs employee_id)
        insert information of one staff to the database (needs the object of PersonInfo)
        update information of specified staff (needs the object of PersonInfo)
        delete information of specified staff (needs employee_id)

- InfoRegister.py
    it is a class (not necessary to be a class)
    
    function: register information of staff to database (needs photo path, name, id, sex)