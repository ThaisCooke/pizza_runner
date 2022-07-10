# Pizzarunner
Week 2 of 8 Week SQL challenge

-- As I did in the first week of the challenge, I created the tables on Microsoft SQL Server

This is part of the 8 week SQL Challenge, by Danny Ma. You can find his challenge here:
https://8weeksqlchallenge.com/case-study-2/

    CREATE TABLE customer_orders (
      "order_id" INTEGER,
      "customer_id" INTEGER,
      "pizza_id" INTEGER,
      "exclusions" VARCHAR(4),
      "extras" VARCHAR(4),
      "order_time" DATETIME DEFAULT CURRENT_TIMESTAMP
    );


      INSERT INTO customer_orders
      ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
    VALUES
    ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
    ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
    ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
    ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
    ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
    ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
    ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
    ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
    ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
    ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
    ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
    ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
    ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
    ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49');    

    CREATE TABLE runners (
    "runner_id" INTEGER,
    "registration_date" DATE
    );
    INSERT INTO runners
    ("runner_id", "registration_date")
    VALUES
      (1, '2021-01-01'),
      (2, '2021-01-03'),
      (3, '2021-01-08'),
      (4, '2021-01-15');

    CREATE TABLE runner_orders (
      "order_id" INTEGER,
      "runner_id" INTEGER,
      "pickup_time" VARCHAR(19),
      "distance" VARCHAR(7),
      "duration" VARCHAR(10),
      "cancellation" VARCHAR(23)
      );

      INSERT INTO runner_orders
      ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
      VALUES
      ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
      ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
      ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
      ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
      ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
      ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
      ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
      ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
      ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
      ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null');


      CREATE TABLE pizza_names (
      "pizza_id" INTEGER,
      "pizza_name" TEXT
      );

    INSERT INTO pizza_names
      ("pizza_id", "pizza_name")
    VALUES
      (1, 'Meatlovers'),
      (2, 'Vegetarian');

      CREATE TABLE pizza_recipes (
      "pizza_id" INTEGER,
      "toppings" TEXT
    );
      INSERT INTO pizza_recipes
      ("pizza_id", "toppings")
      VALUES
      (1, '1, 2, 3, 4, 5, 6, 8, 10'),
      (2, '4, 6, 7, 9, 11, 12');

      CREATE TABLE pizza_toppings (
      "topping_id" INTEGER,
      "topping_name" TEXT
    );
    INSERT INTO pizza_toppings
      ("topping_id", "topping_name")
      VALUES
      (1, 'Bacon'),
      (2, 'BBQ Sauce'),
      (3, 'Beef'),
      (4, 'Cheese'),
      (5, 'Chicken'),
      (6, 'Mushrooms'),
      (7, 'Onions'),
     (8, 'Pepperoni'),
      (9, 'Peppers'),
      (10, 'Salami'),
      (11, 'Tomatoes'),
      (12, 'Tomato Sauce');
      
  
  
  ## When inspecting the tables, there were a lot of "Null" values. I started by cleaning the tables before answering the questions.
  
  -- Cleaning "NULL" and "Null" values from runner_orders table:
  
        UPDATE runner_orders
        SET cancellation = ' '
        WHERE cancellation IS NULL

        UPDATE runner_orders
        SET cancellation = ' '
        WHERE cancellation = 'null'

        UPDATE runner_orders
        SET duration = ' '
        WHERE duration = 'null'

        UPDATE runner_orders
        SET distance = ' '
        WHERE distance = 'null'

        UPDATE runner_orders
        SET pickup_time = ' '
        WHERE pickup_time = 'null'
        
 
 -- Cleaning "NULL" and "Null" values from customer_orders table:
 
        UPDATE customer_orders
        SET exclusions = ' '
        WHERE exclusions = 'null'

        UPDATE customer_orders
        SET extras = ' '
        WHERE extras = 'null'

        UPDATE customer_orders
        SET extras = ' '
        WHERE extras IS NULL
  
  
  ### A. Pizza Metrics
  
  **Question 1**: How many pizzas were ordered?
  
        number_of_orders
            14
  
  
  -- Using the COUNT function:
  
        SELECT COUNT (*) AS number_of_orders
        FROM customer_orders
        
 
 **Question 2**: How many unique customer orders were made?
 
        customer_id	        unique_orders
               101	            3
               102	            2
               103	            2
               104	            2
               105	            1
             
             
 -- I used the functions COUNT DISTINCT and GROUP BY:
 
        SELECT customer_id, COUNT (Distinct order_id) AS unique_orders
        FROM customer_orders
        GROUP BY customer_id
        
        
**Question 3**: How many successful orders were delivered by each runner?

        runner_id	successful_orders
            1	            4
            2	            3
            3	            1
            
            
 -- I used the functions COUNT, WHERE and GROUP BY :
 
        SELECT runner_id, COUNT (order_id) AS successful_orders
        FROM runner_orders
        WHERE cancellation = ' '
        GROUP BY runner_id
        

**Question 4**: How many of each type of pizza was delivered?

        Type_of_pizza	    Number_of_orders
           Meatlovers	        10
           Vegetarian	        4


-- First, I joined the tables pizza_names and customer_orders:

        SELECT pizza_name, order_id, customer_id
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        

-- Then I used COUNT function, however, I was getting an error message:

*Msg 306, Level 16, State 2, Line 16 The text, ntext, and image data types cannot be compared or sorted, except when using IS NULL or LIKE operator*


-- It turned out that the pizza_names column was in **text**, while the other columns from the table customer_orders were in **varchar**. I wrote the same query, using the CAST function:

        SELECT CAST(pizza_name AS VARCHAR(100)) AS Type_of_pizza, COUNT (CAST (pizza_name AS VARCHAR(100))) Number_of_orders
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        GROUP BY CAST (pizza_name AS VARCHAR(100))
        
        
 -- That query gave me the desired result
 
 
 **Question 5**: How many Vegetarian and Meatlovers were ordered by each customer?
 
        customer_id	        Number_of_pizzas	        Type_of_pizza
            101	                    2	                    Meatlovers
            101	                    1	                    Vegetarian
            102	                    2	                    Meatlovers
            102	                    1	                    Vegetarian
            103	                    3	                    Meatlovers
            103	                    1	                    Vegetarian
            104	                    3	                    Meatlovers
            105	                    1	                    Vegetarian
 
 -- First, I joined the tables pizza_names and customer_orders:
 
        SELECT pizza_name, order_id, customer_id
        FROM pizza_names
        INNER JOIN customer_orders
        ON pizza_names.pizza_id = customer_orders.pizza_id
        
-- Then, I used the functions COUNT, GROUP BY and ORDER BY (remembering to use CAST for pizza_name column)


           SELECT customer_id, COUNT (*) AS Number_of_pizzas, CAST (pizza_name AS VARCHAR(100)) AS Type_of_pizza
		   FROM pizza_names
           INNER JOIN customer_orders
           ON pizza_names.pizza_id = customer_orders.pizza_id
		   GROUP BY customer_id, CAST (pizza_name AS VARCHAR(100))
		   ORDER BY customer_id



**Question 6**: What was the maximum number of pizzas delivered in a single order?







  

  
  
  
  
  
