#include "date.h"

using namespace std;

Date::Date() {
    time_t rawtime;
    struct tm* timeinfo;
    time (&rawtime);
    timeinfo = localtime(&rawtime);

    year = 1900 + timeinfo->tm_year;
    month = timeinfo->tm_mon + 1;
    day = timeinfo->tm_mday;
    }

bool Date::isValidDate(const int&y, const int&m, const int&d) {
    time_t rawtime;
    struct tm* timeinfo;
    time (&rawtime);
    timeinfo = localtime(&rawtime);
    unsigned short daysInMonth[] = {0,31,28,31,30,31,30,31,31,30,31,30,31} ;
    if(y == 0 or y < 1900 or y < timeinfo->tm_year or m < 1 or day < 1 or m > 12){
        return false;
    }

    if(m == 2 and (year %4 == 0) and (year%100!=0 or year%400==0)){
        daysInMonth[2]++;
    }
    if(d > daysInMonth[m])
    {
        return false;
    }
    return true;
    }

Date::Date(Date&d) : year(d.year), month(d.month), day(d.day) { }

Date::Date(const int&y, const int&m, const int&d) { ///Si la fecha no es valida, entonces copia la fecha del sistema
    Date();
    if(isValidDate(y,m,d)){
        year = y;
        month = m;
        day = d;
    }
}

int Date::getYear() {
    return year;
    }

int Date::getMonth() {
    return month;
    }

int Date::getDay() {
    return day;
    }

void Date::setYear(const int&y) {
    if(isValidDate(y,month,day)){
        year = y;
    }
}

void Date::setMonth(const int&m) {
    if(isValidDate(year,m,day)){
        month = m;
    }
}

void Date::setDay(const int&d) {
    if(isValidDate(year, month, d)){
        day = d;
    }
}

string Date::toString() {
    char myDate[11];
    sprintf(myDate, "%04i/%02i/%02i" ,year,month,day);
    return myDate;
    }

Date& Date::operator = (const Date&d) {
    year = d.year;
    month = d.month;
    day = d.day;

    return *this;
    }

bool Date::operator == (Date&d) {
    return toInt() == d.toInt();
    }

bool Date::operator != (Date&d) {
    return this ->toString() != d.toString();
    }

bool Date::operator < (Date&d) {
    return this ->toString() < d.toString();
    }

bool Date::operator <= (Date&d) {
    return this->toString() <= d.toString();
    }

bool Date::operator > (Date&d) {
    return this->toString() > d.toString();
    }

bool Date::operator >= (Date&d) {
    return this->toString() >= d.toString();
    }

int Date::toInt(){
    return year*1000+month*100+year;
}

ostream& operator << (ostream& os, Date&d){
    os << d.toString();
    return os;
}

istream& operator >> (istream&is, Date&d)
{
    string myStr;
    getline(is, myStr,'/');
    d.year = atoi(myStr.c_str());
    getline(is, myStr,'/');
    d.month = atoi(myStr.c_str());
    getline(is, myStr,'\n');
    d.day = atoi(myStr.c_str());


    return is;
}

