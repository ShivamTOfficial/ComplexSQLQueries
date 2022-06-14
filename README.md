# Fetch Duplicate Records
This is a commonly asked interview question and a challenging one, I came up with 3 variants to this question

A. Find all the duplicate values</br>

    SELECT *
    FROM users u
    WHERE u.user_id NOT IN (
                            SELECT MIN(user_id) AS user_id
                            FROM users
                            GROUP BY user_name
                            ORDER BY user_id
                            );

B. Fetch all duplicate records</br>

    WITH duplicate_cte AS 
                        (SELECT user_name, email, COUNT(*) AS cnt 
                         FROM users 
                         GROUP BY user_name, email
                        )
    SELECT user_id, u.user_name, u.email 
    FROM users u JOIN duplicate_cte d 
      ON u.user_name = d.user_name 
         AND u.email  = d.email 
    WHERE cnt > 1;

C. Fetch minimum no. of records which when all removed, would remove all duplicacy 

    SELECT user_name, email
    FROM users u
    WHERE u.user_id NOT IN (
                            SELECT ROW_NUMBER()
                            FROM users
                            GROUP BY user_name
                            ORDER BY user_id
                            );
