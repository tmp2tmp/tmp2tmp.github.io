---
layout: page
title: mdtest.md
---

{% highlight ruby %}
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

