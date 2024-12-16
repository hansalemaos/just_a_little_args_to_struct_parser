# Simple args to struct parser

```cpp
#include "parseargstostruct.h"

typedef struct parsed_args
{
    std::string this_file;
    std::string device;
    int64_t screen_width;
    int64_t screen_height;
    int64_t x;
    int64_t y;
    int64_t sleep_time_between_actions;
    double sleep_time_between_actions_as_double;
    std::vector<int64_t> some_int_values;
    std::vector<double> some_double_values;
    std::vector<std::pair<int64_t, int64_t>> some_pair_int_values;
    std::vector<std::pair<double, double>> some_pair_double_values;

    bool just_a_bool_test1;
    bool just_a_bool_test2;
    bool debug_mode;
    bool _device;
    bool _screen_width;
    bool _screen_height;
    bool _x;
    bool _y;
    bool _sleep_time_between_actions;
    bool _sleep_time_between_actions_as_double;
    bool _debug_mode;
    bool _some_int_values;
    bool _some_double_values;
    bool _just_a_bool_test1;
    bool _just_a_bool_test2;
    bool _some_pair_int_values;
    bool _some_pair_double_values;

} p_args;

template <typename T, typename U> std::ostream &operator<<(std::ostream &os, std::vector<std::pair<T, U>> &v)
{
    if (v.size() == 0)
    {
        os << "[]";
        return os;
    }
    auto it{v.begin()};

    os << '[';
    while (it != v.end() - 1)
    {
        os << "[" << it->first << "," << it->second << "]";
        os << ",";

        it++;
    }
    os << "[" << it->first << "," << it->second << "]";
    os << ']';

    return os;
}

template <typename T> std::ostream &operator<<(std::ostream &os, std::vector<T> &v)
{
    if (v.size() == 0)
    {
        os << "[]";
        return os;
    }
    auto it{v.begin()};

    os << '[';
    while (it != v.end() - 1)
    {
        os << *it;
        os << ",";

        it++;
    }
    os << *it;
    os << ']';

    return os;
}
std::ostream &operator<<(std::ostream &os, parsed_args &args)
{
    os << "this_file:\t" << args.this_file << std::endl;
    os << "device:\t\t" << args.device << "\tpassed: " << args._device << std::endl;
    os << "screen_width:\t" << args.screen_width << "\tpassed: " << args._screen_width << std::endl;
    os << "screen_height:\t" << args.screen_height << "\tpassed: " << args._screen_height << std::endl;
    os << "x:\t\t" << args.x << "\tpassed: " << args._x << std::endl;
    os << "y:\t\t" << args.y << "\tpassed: " << args._y << std::endl;
    os << "sleep_time_between_actions:\t" << args.sleep_time_between_actions

       << "\tpassed: " << args._sleep_time_between_actions << std::endl;
    os << "sleep_time_between_actions_as_double:\t" << args.sleep_time_between_actions_as_double
       << "\tpassed: " << args._sleep_time_between_actions_as_double << std::endl;
    os << "debug_mode:\t" << args.debug_mode << "\tpassed: " << args._debug_mode << std::endl;
    os << "some_int_values:\t" << args.some_int_values << "\tpassed: " << args._some_int_values << std::endl;
    os << "some_double_values:\t" << args.some_double_values << "\tpassed: " << args._some_double_values << std::endl;
    os << "just_a_bool_test1:\t" << args.just_a_bool_test1 << "\tpassed: " << args._just_a_bool_test1 << std::endl;
    os << "just_a_bool_test2:\t" << args.just_a_bool_test2 << "\tpassed: " << args._just_a_bool_test2 << std::endl;
    os << "some_pair_int_values:\t" << args.some_pair_int_values << "\tpassed: " << args._some_pair_int_values

       << std::endl;
    os << "some_pair_double_values:\t" << args.some_pair_double_values << "\tpassed: " << args._some_pair_double_values

       << std::endl;
    return os;
}

auto parse_args(int argc, char *argv[])
{
    parsed_args myargs{};
    auto parsed_args_vec{arghelpers::parse_args_to_string_vector(argc, argv)};
    for (auto const &[key, value] : parsed_args_vec)
    {
        if (arghelpers::compare2strings(key, "device"))
        {
            myargs.device = value;
            myargs._device = true;
        }
        else if (arghelpers::compare2strings(key, "__FILE__"))
        {
            myargs.this_file = value;
        }
        else if (arghelpers::compare2strings(key, "screen_width"))
        {
            myargs.screen_width = arghelpers::convert_to_int_at_any_cost(value, arghelpers::MAX_64BIT_INT);
            myargs._screen_width = true;
        }
        else if (arghelpers::compare2strings(key, "screen_height"))
        {
            myargs.screen_height = arghelpers::convert_to_int_at_any_cost(value, arghelpers::MAX_64BIT_INT);
            myargs._screen_height = true;
        }
        else if (arghelpers::compare2strings(key, "x"))
        {
            myargs.x = arghelpers::convert_to_int_at_any_cost(value, arghelpers::MAX_64BIT_INT);
            myargs._x = true;
        }
        else if (arghelpers::compare2strings(key, "y"))
        {
            myargs.y = arghelpers::convert_to_int_at_any_cost(value, arghelpers::MAX_64BIT_INT);
            myargs._y = true;
        }
        else if (arghelpers::compare2strings(key, "sleep_time_between_actions"))
        {
            myargs.sleep_time_between_actions =
                arghelpers::convert_to_int_at_any_cost(value, arghelpers::MAX_64BIT_INT);
            myargs._sleep_time_between_actions = true;
        }
        else if (arghelpers::compare2strings(key, "debug_mode"))
        {
            myargs.debug_mode = arghelpers::to_bool(value);
            myargs._debug_mode = true;
        }
        else if (arghelpers::compare2strings(key, "sleep_time_between_actions_as_double"))
        {
            myargs.sleep_time_between_actions_as_double =
                arghelpers::convert_to_double_at_any_cost(value, arghelpers::MAX_64BIT_INT_AS_DOUBLE);
            myargs._sleep_time_between_actions_as_double = true;
        }
        else if (arghelpers::compare2strings(key, "some_int_values"))
        {
            myargs.some_int_values = arghelpers::parse_ints_to_vector(value, ',', arghelpers::MAX_64BIT_INT);
            myargs._some_int_values = true;
        }
        else if (arghelpers::compare2strings(key, "some_double_values"))
        {
            myargs.some_double_values = arghelpers::parse_doubles_to_vector(value);
            myargs._some_double_values = true;
        }
        else if (arghelpers::compare2strings(key, "just_a_bool_test1"))
        {
            myargs.just_a_bool_test1 = arghelpers::to_bool(value);
            myargs._just_a_bool_test1 = true;
        }
        else if (arghelpers::compare2strings(key, "just_a_bool_test2"))
        {
            myargs.just_a_bool_test2 = arghelpers::to_bool(value);
            myargs._just_a_bool_test2 = true;
        }
        else if (arghelpers::compare2strings(key, "some_pair_int_values"))
        {
            myargs.some_pair_int_values = arghelpers::parse_int_pairs_to_vector(value, arghelpers::MAX_64BIT_INT);
            myargs._some_pair_int_values = true;
        }
        else if (arghelpers::compare2strings(key, "some_pair_double_values"))
        {
            myargs.some_pair_double_values = arghelpers::parse_double_pairs_to_vector(value);
            myargs._some_pair_double_values = true;
        }
    }
    return myargs;
}

int main(int argc, char *argv[])
{
    // zig c++ -std=c++2a -O3 -g0 argparsercpp.cpp
    auto myparsedargs{parse_args(argc, argv)};
    std::cout << myparsedargs << std::endl;
    // a.exe --device="test" --screen_width=1000 --screen_height=1000 --x=100 --y=100 --sleep_time_between_actions=100
    // a.exe --device="test" --screen_width=1000 --screen_height=1000 --x=100 --y=100
    // a.exe --device="test" --screen_width=1000 --screen_height=1000 --some_int_values=1,2,3,4000,129
    // a.exe --device="test" --screen_width=1000 --screen_height=1000 --some_double_values=1.3,2.32,3.1,4000,1.29
    // a.exe --just_a_bool_test1=true --just_a_bool_test2=false
    // a.exe --just_a_bool_test1=1 --just_a_bool_test2=0
    // a.exe --just_a_bool_test1=1 --just_a_bool_test2=0 --some_pair_int_values=(1,2);(3,4);(5,6);(7,8);(9,10)
    // a.exe --just_a_bool_test1=1 --just_a_bool_test2=0 --some_pair_double_values=(1.23,2.231);(3.23,4.11);(5.3,6)
}

```