# String Class

- Overloaded [] operator does not perform any bounds checking.
- Class string does provide **bounds checking** in its member function at, which throws an exception if its argument is an invalid subscript.



(constructor)

assign/operator=



begin/cbegin/end/cend/front

rbegin/crbegin/rend/crend/back



size/length/resize

capacity/reserve

clear

empty



append/operator+=/operator+

push_back/pop_back

insert/erase

replace

swap



c_str/data

copy: string->char*

find/rfind/find_first_of/find_last_of/find_first_not_of/find_last_not_of: pos

substr: begin & length

compare



string::npos



getline