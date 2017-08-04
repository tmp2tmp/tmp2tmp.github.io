---
layout: page
title: mdtest.md
css: /test.css
---

{% highlight ruby linenos %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

```c++
#include <iostream>
using namespace std;
int main() {
   cout << "hello world!" << endl;
}
```
```c++
#include "vane.h"   //required
#include <stdio.h>


/* class hierachy:
    Speak   Ground 
     |       |
    Hello   World 
*/
struct Speak	      { virtual ~Speak(){} };   //polymorphic base is required
struct Hello : Speak  { };


struct Ground	      { virtual ~Ground(){} };  //polymorphic base is required
struct World : Ground { };



///////////////////////////////////////////////////////////////////////////////////
//Fx: defines a set of functions
struct Fx
{
//required:
    using type = void(const char*, Speak*, Ground&);  /* declares the function signatue
							 virtual params : {Speak*,Ground&} ;  two polymorphic params
						      */
    using domains = std::tuple<	      /* declares valid types of arguments */
	    vane::_domain<Speak, Hello>,  //1st virtual argument is valid  when: it is a Speak or a Hello
	    vane::_domain<Ground, World>  //2nd virtual one      is valid  when: it is a Ground or a World
	>;

//optional (individual functions):
    void operator()(const char *s, Speak *p, Ground &q) { printf("%10s --> speak_ground??\n", s);   }
    void operator()(const char *s, Hello *p, World  &q) { printf("%10s --> hello_world   \n", s);   }
};



///////////////////////////////////////////////////////////////////////////////////

template<typename Fx>
void hello_world(Fx *func, const char *p, Speak *s, Ground &g)
{
    (*func) (p, s, g);
}

int main() try 
{
    Fx                     func;        //ordinary function object
    vane::multi_func<Fx>   multi_func;  //multi_func object
    vane::virtual_func<void(const char*, Speak*, Ground&)> *vfunc 
        = &multi_func; //a multi_func is a virtual_func


    Hello hello; 
    World world;

    hello_world(&func,       "func",       &hello, world);
    hello_world(&multi_func, "multi_func", &hello, world);
    hello_world(vfunc,       "vfunc",      &hello, world);

}
catch(const std::exception &e) {
    printf("\n\nexception : %s", e.what());
}


/* output ****************************************************************************
      func --> speak_ground??
multi_func --> hello_world   
     vfunc --> hello_world   
*/
// vim: sts=4:ts=8:smarttab


```
