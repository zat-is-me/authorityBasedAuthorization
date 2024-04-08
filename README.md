# Authority Implementation spring Boot 3.2.4
1, Create Authority table in DB (This application is configured to use Postgres DB)
        
    CREATE TABLE authorities (
    id int NOT NULL GENERATED ALWAYS AS IDENTITY,
    customer_id int NOT NULL,
    name varchar(50) NOT NULL,
    PRIMARY KEY (id),
    CONSTRAINT authorities_fk_1 FOREIGN KEY (customer_id) REFERENCES customer (customer_id)
    );

        INSERT INTO public.authorities(customer_id, name)
    VALUES (1, 'VIEWACCOUNT');
    
    INSERT INTO public.authorities(customer_id, name)
    VALUES (1, 'VIEWCARDS');
    
    INSERT INTO public.authorities(customer_id, name)
    VALUES (1, 'VIEWLOANS');
    
    INSERT INTO public.authorities(customer_id, name)
    VALUES (1, 'VIEWBALANCE');

2, Add Authority entity class 
3, Implement Repository interface to communicate with DB
4, Then apply authority filter in the security filter chain bean. 
            
    ...
    .authorizeHttpRequests(auth -> auth
                    .requestMatchers("/api/v1/contact",
                            "/api/v1/register",
                            "/api/v1/notices")
                    .permitAll()
                    .requestMatchers("/api/v1/user")
                    .authenticated()
                    .requestMatchers("/api/v1/myAccount").hasAuthority("VIEWACCOUNT")
                    .requestMatchers("/api/v1/myBalance").hasAnyAuthority("VIEWACCOUNT","VIEWBALANCE")
                    .requestMatchers("/api/v1/myLoans").hasAuthority("VIEWLOANS")
                    .requestMatchers("/api/v1/myCards").hasAuthority("VIEWCARDS")
            )
    ...
5, Finally if it does not work clone this project and run.

6, Follow me on https://www.linkedin.com/in/tatekahmed/ 