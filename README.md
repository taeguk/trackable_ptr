# trackable_ptr
Trackable pointer. When trackable object moved/destroyed, trackers updated with new object's pointer.

[Live example](http://coliru.stacked-crooked.com/a/c6a2e71ea86f8902)

Allow to have stable pointer on stack allocated movable object (at least in single-threaded environment).
Like:

    struct Data{
        int x,y,z;
    };
    using TData = Trackable<Data>;

    // Data lies in the continuous space
    std::vector< TData > list1;

    TrackablePtr<Data> makeData(){
        list1.emplace_back();
        return {list1.back()};
    }


    TrackablePtr<Data> data = makeData();

        list1.emplace_back();
        list1.emplace_back();
        list1.emplace_back();
        list1.emplace_back();
        // list 1 now uses another memory chuck. All pointers/iterators invalidated.

    // data still alive and accessible;
    std::cout << data->x;



Does not increase object lifetime (like shared_ptr)

Kinda replacement for weak_ptr in terms of object aliveness track.



### Overhead
 * 1 ptr for Trackable
 * 3 ptr for Tracker (TrackablePtr)

 * O(n) complexity for moving/destroying Trackable. Where n = trackers count.