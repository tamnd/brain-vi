---
title: "CF 102775B - \u041a\u0430\u043b\u0435\u043d\u0434\u0430\u0440\u0438"
description: "Nhiệm vụ là so sánh hai hệ thống lịch trong cùng một thời điểm. Đầu vào cung cấp một ngày hợp lệ được viết theo lịch Gregorian, với ngày, tháng và năm. Chúng ta cần xác định lịch Julian hiển thị sớm hơn bao nhiêu ngày cho cùng ngày đó."
date: "2026-07-27T20:36:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 56
verified: true
draft: false
---

[CF 102775B - \u041a\u0430\u043b\u0435\u043d\u0434\u0430\u0440\u0438](https://codeforces.com/problemset/problem/102775/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là so sánh hai hệ thống lịch trong cùng một thời điểm. Đầu vào cung cấp một ngày hợp lệ được viết theo lịch Gregorian, với ngày, tháng và năm. Chúng ta cần xác định lịch Julian hiển thị sớm hơn bao nhiêu ngày cho cùng ngày đó. Nói cách khác, chúng ta cần số ngày mà lịch Julian nằm sau lịch Gregorian. 

Các năm trong phạm vi đầu vào là từ 1582 đến 2500. Đây là một phạm vi rất nhỏ đối với mô phỏng ngày tháng, nhưng thách thức không phải là số lượng ngày tháng. Thách thức là xử lý những thời điểm chính xác khi các quy tắc của năm nhuận khác nhau. Một giải pháp kiểm tra hàng ngày sẽ thực hiện hàng trăm nghìn thao tác, điều này vẫn được chấp nhận ở đây, nhưng nó che giấu ý tưởng thực tế và dễ mắc sai sót hơn khi chuyển đổi lịch. Một giải pháp dựa trên công thức chỉ cần kiểm tra vài thế kỷ và là thời gian không đổi. 

Nguồn sai lầm chính là cho rằng sự khác biệt luôn là một con số cố định. Ví dụ, vào ngày 15 tháng 11 năm 1582, sự khác biệt là 10 ngày, vì lịch Gregory vừa thay thế lịch Julian bằng một sự điều chỉnh mười ngày. Tuy nhiên, đến ngày 1 tháng 3 năm 1700, sự khác biệt là 11 ngày. Lịch Julian coi năm 1700 là năm nhuận, trong khi lịch Gregory thì không, vì vậy lịch Julian có thêm một ngày. 

Một trường hợp ranh giới khác là ngày chính xác trước khi chênh lệch nhảy vọt xuất hiện. Đối với đầu vào`28 2 1700`, đầu ra là`10`. Việc thực hiện bất cẩn làm tăng chênh lệch cho cả năm 1700 sẽ in sai`11`. 

Năm 2000 là một cái bẫy phổ biến khác. Đối với đầu vào`1 3 2000`, đầu ra vẫn là`13`, vì năm 2000 là năm nhuận trong cả hai dương lịch. Một giải pháp chỉ kiểm tra xem một năm có chia hết cho 100 hay không sẽ cộng thêm một ngày một cách không chính xác. 

Ranh giới cuối cùng là gần năm tối đa. Đối với đầu vào`17 3 2500`, đầu ra là`17`. Lịch Julian coi năm 2500 là năm nhuận, trong khi lịch Gregory thì không, và ngày bổ sung sẽ xuất hiện sau khi tháng Hai kết thúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng lịch từng ngày. Bắt đầu từ ngày Gregory đầu tiên có lịch khác nhau, chúng ta có thể chuyển tiếp từng ngày một và duy trì ngày Julian và ngày Gregorian riêng biệt. Bất cứ khi nào một lịch có ngày nhuận còn lịch kia thì không, sự khác biệt sẽ thay đổi. Điều này hiệu quả vì mọi chuyển đổi ngày riêng lẻ đều có thể được mô hình hóa chính xác. 

Vấn đề với phương pháp này là nó giải quyết được một vấn đề lớn hơn nhiều so với mức cần thiết. Phạm vi đầu vào bao gồm hơn chín trăm năm, tức là hơn ba trăm nghìn ngày. Mặc dù phạm vi cụ thể này không lớn nhưng phương pháp mô phỏng sẽ thực hiện những công việc không cần thiết và làm cho logic chuyển đổi lịch trở nên phức tạp hơn. 

Điều quan trọng cần lưu ý là các lịch chỉ khác nhau do quy luật của năm nhuận. Cả hai lịch đều có cùng số ngày trong các năm bình thường, do đó độ lệch chỉ thay đổi sau những năm mà một lịch có thêm ngày tháng Hai còn lịch kia thì không. 

Khoảng thời gian bù đắp ban đầu sau cải cách Gregorian là 10 ngày. Từ đó, chúng ta chỉ cần tính những năm thế kỷ Gregorian là năm nhuận trong lịch Julian chứ không phải trong lịch Gregorian. Những năm này là 1700, 1800, 1900, 2100, 2200, 2300 và 2500. Mỗi năm đóng góp thêm một ngày vào lịch Julian, nhưng chỉ khi ngày nhất định nằm sau tháng Hai của năm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số ngày giữa các ngày) | O(1) | Hoạt động trong phạm vi này, nhưng không cần thiết | 
| Tối ưu | O(phạm vi năm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với mức chênh lệch lịch sử là 10 ngày. Mọi ngày đầu vào hợp lệ đều sau ngày 15 tháng 10 năm 1582, vì vậy lịch Gregory đã đi trước lịch Julian mười ngày. 
2. Đi qua hàng năm từ năm 1583 đến năm đó. Kiểm tra xem năm nay có tạo thêm sự khác biệt giữa các lịch hay không. 
3. Đối với mỗi năm, hãy kiểm tra xem lịch Julian có ngày nhuận trong khi lịch Gregory không. Điều này xảy ra khi năm chia hết cho 100 chứ không chia hết cho 400. Đây chính xác là những năm thế kỷ mà quy tắc Gregorian loại bỏ nhưng quy tắc Julian vẫn giữ nguyên. 
4. Nếu một năm như vậy đã qua ngày nhuận tháng Hai, hãy thêm một vào câu trả lời. Nếu ngày nhập vẫn là tháng 1 hoặc tháng 2 năm đó thì ngày bổ sung chưa xuất hiện nên không được tính. 
5. In offset tích lũy. 

Tại sao nó hoạt động: sự khác biệt duy nhất có thể có giữa các lịch đến từ những ngày nhuận. Những năm bình thường đóng góp chính xác số ngày như nhau cho cả hai hệ thống. Khoản chênh lệch bắt đầu tính đến cải cách Gregorian và mỗi lần điều chỉnh sau đó tương ứng với một ngày nhuận bị thiếu vẫn còn tồn tại trong lịch Julian. Vì chúng ta tính mỗi ngày nhuận như vậy một cách chính xác khi nó trở thành một phần của thời gian đã trôi qua, nên giá trị cuối cùng chính xác là chênh lệch lịch. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    d, m, y = map(int, input().split())

    ans = 10

    for year in range(1583, y + 1):
        if year % 100 == 0 and year % 400 != 0:
            if year < y or (year == y and m > 2):
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình bắt đầu với`ans = 10`bởi vì mỗi ngày đầu vào đều diễn ra sau khi lịch Gregorian được giới thiệu và lần điều chỉnh ban đầu so với lịch Julian luôn là mười ngày. 

Vòng lặp chỉ kiểm tra năm thế kỷ. Năm nhuận bình thường xuất hiện trong cả hai lịch nên không thể ảnh hưởng đến sự khác biệt. Một năm chia hết cho 100 là năm đặc biệt vì lịch Gregory loại bỏ ngày nhuận trừ khi nó cũng chia hết cho 400. 

điều kiện`year < y or (year == y and m > 2)`xử lý ranh giới chính xác của tháng Hai. Ngày nhuận vào ngày 29 tháng 2 chỉ thay đổi độ lệch sau khi ngày đó trôi qua. Đối với một ngày trong tháng 2 hoặc sớm hơn, ngày nhuận bị thiếu của năm đó vẫn chưa ảnh hưởng đến sự khác biệt. 

Phạm vi này rất nhỏ nên việc lặp qua các năm sẽ đơn giản và an toàn hơn so với việc cố gắng nén các phép tính thế kỷ vào một công thức duy nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`15 11 1582`. 

| Năm kiểm tra | Phần bù hiện tại | Lý do | 
| --- | --- | --- | 
| Bắt đầu | 10 | Sự khác biệt cải cách Gregorian | 
| Không có năm sau | 10 | Chưa có sự khác biệt bước nhảy vọt nào | 

Ngày này diễn ra ngay sau lần điều chỉnh Gregorian, trước khi xảy ra bất kỳ năm nhuận nào trong thế kỷ không khớp. Kết quả vẫn còn 10 ngày. 

Đối với mẫu thứ hai, đầu vào là`31 1 1918`. 

| Năm kiểm tra | Phần bù hiện tại | Lý do | 
| --- | --- | --- | 
| Bắt đầu | 10 | Sự khác biệt cải cách Gregorian | 
| 1700 | 11 | Julian có thêm một ngày nhuận | 
| 1800 | 12 | Julian có thêm một ngày nhuận | 
| 1900 | 13 | Julian có thêm một ngày nhuận | 
| Cuối cùng | 13 | Tháng 1 năm 1918 sau tất cả những thay đổi trước đó | 

Dấu vết này cho thấy tại sao câu trả lời phụ thuộc vào năm và tháng của ngày chứ không chỉ năm. Sự khác biệt nhảy vọt so với năm 1900 đã tồn tại vì ngày nhập muộn hơn nhiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(y - 1582) | Thuật toán kiểm tra mỗi năm một lần | 
| Không gian | O(1) | Chỉ các biến câu trả lời và vòng lặp hiện tại được lưu trữ | 

Năm tối đa là 2500, do đó vòng lặp chạy ít hơn một nghìn lần lặp. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    d, m, y = map(int, input().split())

    ans = 10
    for year in range(1583, y + 1):
        if year % 100 == 0 and year % 400 != 0:
            if year < y or (year == y and m > 2):
                ans += 1

    return str(ans)

# provided samples
assert solution("15 11 1582\n") == "10", "sample 1"
assert solution("31 1 1918\n") == "13", "sample 2"

# custom cases
assert solution("15 10 1582\n") == "10", "first valid Gregorian date"
assert solution("1 3 1700\n") == "11", "first century leap mismatch"
assert solution("1 3 2000\n") == "13", "Gregorian 400-year leap rule"
assert solution("17 3 2500\n") == "17", "maximum boundary date"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`15 10 1582`| 10 | Ranh giới cải cách Gregorian bắt đầu | 
|`1 3 1700`| 11 | Năm đầu tiên lịch lại phân kỳ | 
|`1 3 2000`| 13 | Quy tắc Gregorian đặc biệt chia hết cho 400 năm | 
|`17 3 2500`| 17 | Năm tối đa và mức tăng bù đắp cuối cùng có thể | 

## Vỏ cạnh 

cho`15 10 1582`, thuật toán bắt đầu với độ lệch là 10 và không tìm thấy năm nhuận thế kỷ nào không khớp hoàn toàn. Đầu ra là 10, khớp với hiệu chỉnh Gregorian ngay lập tức. 

Vì`28 2 1700`, vòng lặp phát hiện rằng 1700 là năm thế kỷ mà Julian có ngày nhuận còn Gregorian thì không, nhưng điều kiện`m > 2`là sai. Đáp án vẫn là 10 vì ngày 29/2 chưa ảnh hưởng tới chênh lệch lịch. 

Vì`1 3 1700`, cùng năm đó đã qua tháng Hai. Thuật toán thêm một ngày vào năm 1700, thay đổi câu trả lời từ 10 thành 11. Điều này xử lý chính xác điểm chuyển tiếp một cách chính xác. 

Vì`1 3 2000`, năm đó chia hết cho 400 nên cả hai dương lịch đều là năm nhuận. Thuật toán không tính nó, để lại chênh lệch tích lũy ở mức 13. 

cho`17 3 2500`, thuật toán sẽ tính tất cả các năm trong thế kỷ không khớp trước đó và cũng tính 2500 vì ngày đó là sau tháng Hai. Câu trả lời cuối cùng là 17, đây là sự khác biệt lớn nhất có thể có trong phạm vi đầu vào.
