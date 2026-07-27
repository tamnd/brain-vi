---
title: "CF 102775G - \u041c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u0447\u0435\u0441\u043a\u043e\u0435 \u0440\u0430\u0432\u0435\u043d\u0441\u0442\u0432\u043e"
description: "Nhiệm vụ là chia một số tự nhiên N cho trước thành nhiều số nguyên dương. Mỗi phần của phép chia phải có tổng các chữ số thập phân bằng nhau. Số lượng bộ phận ít nhất phải là hai, không thể đạt tới N và không thể vượt quá một triệu."
date: "2026-07-27T20:40:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 72
verified: true
draft: false
---

[CF 102775G - \u041c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u0447\u0435\u0441\u043a\u043e\u0435 \u0440\u0430\u0432\u0435\u043d\u0441\u0442\u0432\u043e](https://codeforces.com/problemset/problem/102775/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chia một số tự nhiên cho trước`N`thành nhiều số nguyên dương. Mỗi phần của phép chia phải có tổng các chữ số thập phân bằng nhau. Số lượng phần ít nhất phải là hai, không thể đạt tới`N`chính nó, và không thể vượt quá một triệu. Nếu sự phân chia như vậy tồn tại, chúng tôi sẽ in số lượng bộ phận và bản thân các bộ phận đó. Ngược lại, chúng tôi in`-1`. 

giới hạn`N <= 10^18`ngay lập tức loại trừ việc thử các lệnh triệu tập có thể có hoặc tìm kiếm trên các phân vùng. Thậm chí lặp lại tất cả các giá trị lên đến`N`là không thể vì đầu vào có thể chứa một số khoảng một triệu. Quan sát hữu ích chỉ phụ thuộc vào số chữ số, bởi vì`N`có nhiều nhất 19 chữ số thập phân. 

Những trường hợp khó không phải là những giá trị lớn mà là những giá trị có rất ít khả năng phân tách. Ví dụ,`N = 1`không thể tách được vì không có cách nào để tạo ra ít nhất hai triệu hồi dương. Việc triển khai bất cẩn luôn in các chữ số thập phân dưới dạng lũy ​​thừa của mười sẽ tạo ra một lệnh triệu tập cho trường hợp này. 

Vì`N = 10`, việc phân tách chữ số thập phân chỉ đưa ra một lệnh triệu tập,`10`, không hợp lệ. Đầu ra đúng là`2`theo sau là`5 5`, bởi vì cả hai lệnh đều có tổng chữ số`5`. Việc thực hiện bất cẩn chỉ đếm các chữ số thập phân sẽ bỏ lỡ tất cả lũy thừa của mười. 

Vì`N = 2`, việc phân tách thành các bản sao có giá trị chữ số hoạt động chính xác:`1 + 1`. Cả hai số đều có tổng chữ số`1`, và số lần triệu tập là hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng tìm một số tổng chữ số`S`, tạo số có tổng chữ số`S`và tìm kiếm sự kết hợp cộng với`N`. Điều này đúng vì mọi câu trả lời hợp lệ đều phải sử dụng các số có tổng một chữ số chung. Tuy nhiên, không gian tìm kiếm là rất lớn. Thậm chí tạo ra tất cả các ứng cử viên dưới đây`10^18`là không thể, và việc tìm kiếm phân vùng sẽ có nhiều khả năng hơn mức có thể xử lý trong một giây. 

Cấu trúc biểu diễn số thập phân đưa ra một lộ trình đơn giản hơn nhiều. Bất kỳ chữ số nào`d`ở vị trí`10^k`có thể được xem như`d`bản sao của số`10^k`. Mỗi bản sao như vậy có tổng chữ số`1`. Ví dụ,`352`trở thành`3 * 100 + 5 * 10 + 2 * 1`, có thể được viết dưới dạng số có tổng một chữ số:`100, 100, 100, 10, 10, 10, 10, 10, 1, 1`Mọi triệu hồi đều có tổng các chữ số giống nhau, vì vậy đây là một cách xây dựng hợp lệ bất cứ khi nào tổng các chữ số thập phân của`N`ít nhất là hai. Số số hạng được tạo ra chính xác bằng tổng các chữ số của`N`, nhiều nhất là`171`đối với các giới hạn đã cho. 

Lỗi duy nhất của cách xây dựng này xảy ra khi tổng các chữ số là một. Những con số như vậy chính xác là lũy thừa của mười. Vì`N = 1`, không có câu trả lời tồn tại. Đối với bất kỳ lũy thừa mười nào khác, việc chia nó thành hai nửa bằng nhau sẽ có tác dụng. số`10^k / 2`có tổng chữ số`5`, nên hai nửa luôn thỏa mãn yêu cầu. 

Lực lượng vũ phu hoạt động vì nó tìm kiếm cùng một thuộc tính mà chúng ta khai thác, các tổng chữ số bằng nhau, nhưng nó cố gắng khám phá cấu trúc mà biểu diễn thập phân đã cung cấp cho chúng ta. Nhận xét rằng lũy ​​thừa của số mười tự nhiên có tổng chữ số bằng một cho phép chúng ta xây dựng hầu hết mọi câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng phân vùng có thể có) | O(số lượng ứng viên) | Quá chậm | 
| Tối ưu | O(số chữ số + kích thước câu trả lời) | O(kích thước câu trả lời) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các chữ số thập phân của`N`. Điều này cho chúng ta biết liệu việc phân tách chữ số đơn giản sẽ chứa một hay nhiều lệnh triệu tập. 
2. Nếu`N`là`1`, in`-1`. Không có cách nào tạo ra hai số dương có tổng bằng`1`. 
3. Nếu tổng các chữ số của`N`là`1`, in hai bản sao của`N / 2`. Những giá trị duy nhất như vậy là lũy thừa của mười và mọi lũy thừa của mười lớn hơn một đều là số chẵn. Mỗi nửa có tổng chữ số`5`. 
4. Nếu không, hãy quét các chữ số thập phân của`N`từ phải sang trái. Đối với mỗi chữ số`d`ở vị trí`10^k`, thêm vào`d`bản sao của`10^k`để trả lời. 

Lý do điều này hoạt động là vì mỗi giá trị được tạo chỉ có một chữ số khác 0, vì vậy mọi giá trị được tạo đều có tổng chữ số`1`. Việc thêm các bản sao sẽ xây dựng lại chính xác số thập phân ban đầu. 
5. In số lượng lệnh triệu tập được tạo và bản thân lệnh triệu tập đó. 

Bất biến đằng sau việc xây dựng là mọi lệnh triệu tập được tạo ra đều có tổng chữ số giống nhau. Trường hợp đặc biệt duy nhất là khi phân tách chữ số tự nhiên sẽ chứa một triệu hồi, điều này xảy ra chính xác với lũy thừa mười. Việc xử lý riêng biệt các số đó sẽ giữ nguyên tính bất biến trong khi vẫn đáp ứng yêu cầu có ít nhất hai phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(x):
    return sum(map(int, str(x)))

def solve():
    n = int(input())

    if n == 1:
        print(-1)
        return

    if digit_sum(n) == 1:
        print(2)
        print(n // 2, n // 2)
        return

    ans = []
    x = n
    power = 1

    while x > 0:
        digit = x % 10
        for _ in range(digit):
            ans.append(power)
        power *= 10
        x //= 10

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý đầu vào không thể duy nhất. Điều kiện được kiểm tra trước trường hợp lũy thừa mười vì`1`cũng có tổng chữ số bằng một nhưng không thể chia nó thành hai số nguyên dương. 

Nhánh thứ hai xử lý tất cả các lũy thừa của mười lớn hơn một. Phép chia là chính xác vì các số này là số chẵn và các nửa kết quả có tổng các chữ số bằng nhau. 

Vòng lặp chính trích xuất các chữ số từ phải sang trái.`power`luôn lưu trữ giá trị vị trí thập phân hiện tại. Nếu chữ số hiện tại là`d`, nối thêm`d`bản sao của`power`đóng góp chính xác số tiền bằng chữ số đó đóng góp trong số ban đầu. Kích thước câu trả lời vẫn rất nhỏ vì tổng chữ số thập phân tối đa có thể có cho phạm vi đầu vào này là`171`. 

Số nguyên Python không bị tràn, vì vậy một nửa`10^18`và tất cả các giá trị trung gian đều an toàn. Vòng lặp cũng tránh chuyển đổi số thành danh sách các chữ số, điều này giúp quá trình triển khai được diễn ra trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | Giá trị hiện tại | Tổng chữ số | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Kiểm tra trường hợp không thể | -1 | 

Thuật toán ngay lập tức từ chối đầu vào vì người ta không thể tạo ít nhất hai lệnh triệu dương từ`1`. 

### Ví dụ 2 

đầu vào:```
352
```| Bước | Chữ số hiện tại | Quyền lực | Bản sao đã thêm | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1, 1 | 1, 1 | 
| 2 | 5 | 10 | 10, 10, 10, 10, 10 | 1, 1, 10, 10, 10, 10, 10 | 
| 3 | 3 | 100 | 100, 100, 100 | 1, 1, 10, 10, 10, 10, 10, 100, 100, 100 | 

Các số được tạo ra có tổng là`352`. Mọi phần tử đều có tổng chữ số`1`, do đó bất biến được duy trì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số chữ số + kích thước câu trả lời) | Số này có tối đa 19 chữ số và tối đa 171 lệnh triệu tập được tạo ra. | 
| Không gian | O(kích thước câu trả lời) | Đầu ra được lưu trữ chỉ chứa các lệnh triệu tập được tạo. | 

Thuật toán thực hiện một lượng công việc không đổi cho các giới hạn của bài toán này. Đầu ra lớn nhất có thể chỉ có vài trăm số nguyên, thấp hơn nhiều so với giới hạn một triệu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def digit_sum(x):
        return sum(map(int, str(x)))

    n = int(sys.stdin.readline())

    if n == 1:
        return "-1"

    if digit_sum(n) == 1:
        return f"2\n{n // 2} {n // 2}"

    ans = []
    x = n
    power = 1
    while x > 0:
        d = x % 10
        for _ in range(d):
            ans.append(power)
        power *= 10
        x //= 10

    return str(len(ans)) + "\n" + " ".join(map(str, ans))

def check(inp):
    out = solve_case(inp)
    if out == "-1":
        return out

    lines = out.splitlines()
    m = int(lines[0])
    values = list(map(int, lines[1].split()))

    assert len(values) == m
    assert 2 <= m
    assert all(v > 0 for v in values)
    assert sum(values) == int(inp)
    assert len({sum(map(int, str(v))) for v in values}) == 1
    return out

# provided samples
assert check("1\n") == "-1", "sample 1"
assert check("2\n").splitlines()[0] == "2", "sample 2"

# custom cases
check("10\n")
check("1000000000000000000\n")
check("999999999999999999\n")
check("101010101010101010\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10`| Hai nửa bằng nhau | Khả năng xử lý đặc biệt mạnh mẽ của mười | 
|`1000000000000000000`| Hai nửa lớn hợp lệ | Ranh giới đầu vào tối đa | 
|`999999999999999999`| Nhiều thuật ngữ tổng một chữ số | Xây dựng câu trả lời lớn | 
|`101010101010101010`| Vị trí thập phân lặp lại | Trích xuất chữ số đúng | 

## Vỏ cạnh 

cho`N = 1`, thuật toán đạt đến điều kiện đầu tiên và in`-1`. Không có giá trị dương nào có thể được sử dụng hai lần trong khi tổng của chúng bằng một. 

Vì`N = 10`, việc phân tách chữ số thông thường sẽ chỉ cho kết quả không chính xác`[10]`. Thuật toán phát hiện tổng các chữ số là một, sử dụng trường hợp đặc biệt và xuất ra`5 5`. Cả hai số đều có tổng chữ số năm và cùng tạo thành mười. 

Vì`N = 2`, tổng các chữ số là hai, do đó việc phân tách tạo ra hai bản sao của`1`. Đầu ra chứa hai số tiền có tổng bằng một chữ số và tổng của chúng chính xác là số ban đầu. 

Vì`N = 10^18`, tổng các chữ số là một, do đó thuật toán không cố gắng tạo ra một lệnh triệu đơn lẻ. Nó xuất ra hai bản sao của`500000000000000000`, đáp ứng tất cả các hạn chế trong khi tránh câu trả lời một thành phần không hợp lệ.
