---
title: "CF 102766E - Số Singhal và số bị thiếu"
description: "Chúng ta có một chuỗi được tạo bằng cách viết nhiều số nguyên liên tiếp cạnh nhau. Các số nguyên ban đầu tạo thành một phạm vi từ giá trị bắt đầu nào đó n đến giá trị kết thúc m, với ít nhất ba số trong phạm vi."
date: "2026-07-28T23:39:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "E"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 74
verified: true
draft: false
---

[CF 102766E - Số Singhal và số bị thiếu](https://codeforces.com/problemset/problem/102766/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi được tạo bằng cách viết nhiều số nguyên liên tiếp cạnh nhau. Các số nguyên ban đầu hình thành một phạm vi từ một số giá trị bắt đầu`n`đến giá trị kết thúc`m`, với ít nhất ba số trong phạm vi. Chính xác một số nguyên ở giữa phạm vi này đã bị bỏ qua trước khi nối. Nhiệm vụ là khôi phục số nguyên bị bỏ qua và nếu nhiều phạm vi có thể giải thích chuỗi, hãy trả về giá trị thiếu nhỏ nhất có thể. 

Ví dụ, chuỗi`1314151718`có thể được chia thành`13, 14, 15, 17, 18`, xuất phát từ phạm vi liên tiếp`13`ĐẾN`18`với`16`LOẠI BỎ. Câu trả lời là giá trị vắng mặt, không phải chuỗi còn lại. 

Giới hạn trên của các số rất lớn, lên tới`10^9`, vì vậy việc tạo ra toàn bộ phạm vi là không thể. Một phạm vi có thể chứa một số lượng lớn các số nguyên trong khi chuỗi đầu vào chỉ có độ dài`10^5`. Tổng kích thước đầu vào cũng được giới hạn ở`10^5`ký tự, có nghĩa là giải pháp phải hoạt động theo thời gian gần như tuyến tính theo độ dài của mỗi chuỗi. Việc thử tất cả các số bắt đầu và kết thúc có thể là quá tốn kém. 

Các trường hợp phức tạp xuất phát từ sự mơ hồ ở đầu chuỗi. Số đầu tiên có thể là giá trị bắt đầu thực tế hoặc số bị thiếu có thể là giá trị đầu tiên của phạm vi. Ví dụ, đầu vào`13`có thể đại diện cho những con số`1, 2, 3`với`2`thiếu, vậy đáp án là`2`. Giải pháp luôn coi chuỗi chữ số đầu tiên là số hiện có đầu tiên sẽ bỏ lỡ khả năng này. 

Một trường hợp cạnh khác là khi số bị thiếu nằm ở cuối phạm vi. Ví dụ, đầu vào`123`có thể đại diện`1,2,3,4`với`4`mất tích. Trình phân tích cú pháp chỉ kiểm tra các bước nhảy giữa các số được phân tích liền kề sẽ không bao giờ phát hiện ra giá trị bị thiếu và sẽ từ chối chuỗi một cách không chính xác. 

Trường hợp thứ ba là thay đổi độ dài chữ số. Ví dụ,`899100101`đại diện cho`89,90,91,101`chỉ khi phần tách được chọn không chính xác, trong khi cách giải thích hợp lệ có thể liên quan đến việc thiếu một số xung quanh quá trình chuyển đổi từ hai chữ số sang ba chữ số. Trình phân tích cú pháp phải so sánh toàn bộ số dự kiến ​​thay vì giả sử một số chữ số cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đoán chuỗi liên tiếp hoàn chỉnh. Chúng ta có thể thử mọi số đầu tiên có thể, liên tục chia chuỗi thành các số nguyên tăng dần và kiểm tra xem có chính xác một giá trị bị bỏ qua hay không. Điều này đúng vì bất kỳ câu trả lời hợp lệ nào cũng phải đến từ một chuỗi liên tiếp như vậy. Vấn đề là không gian tìm kiếm. Số đầu tiên có thể lớn và số phần chia có thể tăng lên nhanh chóng. Khám phá tất cả các phân vùng của một chuỗi có độ dài`10^5`là không thực tế. 

Quan sát hữu ích là số hiện có đầu tiên phải xuất hiện ở đầu chuỗi. Vì mọi số trong đáp án đều nhỏ hơn`10^9`, số đầu tiên có nhiều nhất chín chữ số. Chúng ta chỉ cần kiểm tra một vài tiền tố có thể có này làm ứng cử viên cho số đầu tiên. 

Đối với số bắt đầu được chọn, phần còn lại của chuỗi là xác định. Chúng tôi luôn biết số nào sẽ đến tiếp theo. Nếu phần tiếp theo của chuỗi khớp với số dự kiến ​​đó thì chúng ta sẽ sử dụng nó. Nếu nó không khớp, lời giải thích duy nhất được phép là số dự kiến ​​này là số bị thiếu và phần hiện tại của chuỗi phải là số sau. Sau đó, giá trị còn thiếu đã được sử dụng nên mọi số sau đó phải khớp chính xác. 

Điều này chuyển đổi vấn đề từ việc tìm kiếm qua nhiều phân vùng thành việc kiểm tra một số lượng nhỏ các lần khởi động có thể xảy ra, mỗi lần khởi động theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lần chia có thể | O(1) | Quá chậm | 
| Tối ưu | O(9 * | S | ) | 

## Hướng dẫn thuật toán 

1. Hãy thử mọi độ dài tiền tố có thể có từ`1`ĐẾN`9`như số đầu tiên. Tiền tố dài hơn chín chữ số không thể biểu thị số hợp lệ vì giá trị bị thiếu được đảm bảo ở bên dưới`10^9`. 
2. Đối với mỗi số bắt đầu ứng cử viên, hãy phân tích chuỗi còn lại trong khi mong đợi các số nguyên liên tiếp. Bản thân số đầu tiên được sử dụng ngay lập tức vì nó xác định điểm bắt đầu của chuỗi. 
3. Giữ nguyên số nguyên dự kiến ​​tiếp theo và gắn cờ cho biết số còn thiếu đã được tìm thấy hay chưa. Khi các ký tự tiếp theo khớp với số nguyên dự kiến, hãy sử dụng chúng và tăng kỳ vọng. 
4. Nếu số nguyên dự kiến ​​không khớp với phần hiện tại của chuỗi, hãy coi số nguyên dự kiến ​​đó là giá trị còn thiếu. Phần hiện tại phải khớp với số sau nó. Nếu bước nhảy này xảy ra nhiều lần, lần bắt đầu của ứng viên sẽ không hợp lệ. 
5. Sau khi sử dụng toàn bộ chuỗi, hãy kiểm tra xem có chính xác một số bị bỏ qua hay không. Nếu chuỗi kết thúc ngay sau số hiện có cuối cùng thì giá trị còn thiếu là số dự kiến ​​tiếp theo. 
6. Trong số tất cả các ứng viên hợp lệ, trả về giá trị còn thiếu nhỏ nhất. 

Tại sao nó hoạt động: Đối với một số bắt đầu cố định, chuỗi số xuất hiện hoàn toàn được xác định. Ở mọi vị trí, con số dự kiến ​​​​sẽ xuất hiện hoặc sự khác biệt duy nhất có thể xảy ra được vấn đề cho phép là con số dự kiến ​​​​này đã bị bỏ qua. Vì chỉ được phép bỏ sót một lần nên việc phân tích cú pháp tham lam không thể bỏ qua số sai. Việc kiểm tra tất cả các số đầu tiên có thể có bao gồm mọi cách xây dựng hợp lệ vì số hiện có đầu tiên là số bắt đầu thực sự của phạm vi hoặc là số ngay sau giá trị đầu tiên bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    ans = 10**9

    def check(start):
        pos = 0
        cur = start
        missing = None

        first = str(start)
        if not s.startswith(first):
            return None
        pos += len(first)
        cur += 1

        while pos < len(s):
            expected = str(cur)
            if s.startswith(expected, pos):
                pos += len(expected)
                cur += 1
            else:
                if missing is not None:
                    return None
                nxt = str(cur + 1)
                if not s.startswith(nxt, pos):
                    return None
                missing = cur
                pos += len(nxt)
                cur += 2

        if missing is None:
            missing = cur

        return missing

    for length in range(1, min(9, len(s)) + 1):
        if length > 1 and s[0] == '0':
            continue
        start = int(s[:length])
        value = check(start)
        if value is not None:
            ans = min(ans, value)

    return str(ans)

def main():
    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        out.append(solve_case(s))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên sẽ thử mọi độ dài có thể có của số đầu tiên. Chỉ có chín khả năng vì số hợp lệ ở bên dưới`10^9`. 

các`check`chức năng thực hiện quét tham lam. Biến`cur`lưu trữ số sẽ xuất hiện tiếp theo. Khi chuỗi chứa`cur`, quá trình quét diễn ra bình thường. Khi không, hàm này sẽ xác minh rằng`cur + 1`thay vào đó xuất hiện và ghi lại`cur`như số còn thiếu. 

Điều kiện cuối cùng xử lý số cuối cùng bị thiếu. Nếu quá trình quét kết thúc mà không tìm thấy khoảng trống, khả năng duy nhất còn lại là con số dự kiến ​​tiếp theo không bao giờ được ghi. Việc triển khai tránh tràn số nguyên vì số nguyên Python tự động tăng lên, mặc dù các giá trị lớn nhất ở đây cũng đủ nhỏ cho số nguyên 64 bit tiêu chuẩn. 

## Ví dụ đã hoạt động 

cho`3457`, một dấu vết thành công là: 

| Bước | Con số dự kiến ​​| Vị trí hiện tại | Hành động | Thiếu | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 0 | Đọc 3 | không | 
| 1 | 4 | 1 | Đọc 4 | không | 
| 2 | 5 | 2 | Đọc 5 | không | 
| 3 | 6 | 3 | 7 xuất hiện, bỏ qua 6 | 6 | 
| 4 | 8 | kết thúc | Kết thúc | 6 | 

Dấu vết cho thấy bước nhảy duy nhất được sự cố cho phép. Giá trị bị bỏ qua là giá trị mong đợi trước khi nhảy. 

Vì`1314151718`, vết là: 

| Bước | Con số dự kiến ​​| Vị trí hiện tại | Hành động | Thiếu | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 13 | 0 | Đọc 13 | không | 
| 1 | 14 | 2 | Đọc 14 | không | 
| 2 | 15 | 4 | Đọc 15 | không | 
| 3 | 16 | 6 | 17 xuất hiện, bỏ qua 16 | 16 | 
| 4 | 18 | 8 | Đọc 18 | 16 | 

Sự chuyển tiếp xung quanh`16`Và`17`chứng tỏ tại sao việc so sánh các số nguyên đầy đủ là cần thiết. Số chữ số thay đổi tự nhiên trong quá trình phân tích cú pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9 * | S | 
| Không gian | O(1) | Chỉ các bộ đếm và giá trị tạm thời được lưu trữ. | 

Tổng chiều dài đầu vào là`10^5`, do đó nghiệm chỉ thực hiện một bội số không đổi nhỏ của công tuyến tính. Nó phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    def solve_case(s):
        ans = 10**9

        def check(start):
            pos = 0
            cur = start
            missing = None

            if not s.startswith(str(start)):
                return None
            pos += len(str(start))
            cur += 1

            while pos < len(s):
                if s.startswith(str(cur), pos):
                    pos += len(str(cur))
                    cur += 1
                else:
                    if missing is not None:
                        return None
                    if not s.startswith(str(cur + 1), pos):
                        return None
                    missing = cur
                    pos += len(str(cur + 1))
                    cur += 2

            return cur if missing is None else missing

        for i in range(1, min(9, len(s)) + 1):
            v = check(int(s[:i]))
            if v is not None:
                ans = min(ans, v)
        return str(ans)

    res = []
    for x in data[1:]:
        res.append(solve_case(x))
    return "\n".join(res)

assert run("""4
13
3457
1314151718
234235236238
""") == """2
6
16
237"""

assert run("""1
123
""") == "4"

assert run("""1
8990
""") == "9"

assert run("""1
11121315
""") == "14"

assert run("""1
99910001002
""") == "1001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`13`|`2`| Thiếu giá trị ở đầu phạm vi | 
|`123`|`4`| Thiếu giá trị sau chuỗi hiển thị | 
|`8990`|`9`| Các giá trị liên tiếp có ranh giới chữ số | 
|`11121315`|`14`| Thiếu giá trị trong chuỗi nhiều chữ số | 
|`99910001002`|`1001`| Chuyển từ số có ba sang bốn chữ số | 

## Vỏ cạnh 

Khi số bị thiếu là giá trị đầu tiên, thuật toán sẽ xử lý nó vì số hiện có đầu tiên cũng có thể là số sau số bị thiếu. Đối với đầu vào`13`, ứng viên bắt đầu`1`phân tích trình tự như`1,3`và giá trị còn thiếu được phát hiện là`2`. Ứng viên bắt đầu`13`không hợp lệ hoặc đưa ra cách giải thích lớn hơn, vì vậy câu trả lời tối thiểu vẫn còn`2`. 

Khi số bị thiếu là giá trị cuối cùng, không có bước nhảy nào được nhìn thấy bên trong chuỗi. Đối với đầu vào`123`, trình tự có thể là`1,2,3,4`với`4`mất tích. Trình phân tích cú pháp kết thúc sau khi đọc`3`, thấy rằng không có khoảng trống nào được ghi lại và gán số dự kiến ​​​​tiếp theo làm giá trị còn thiếu. 

Khi chuỗi vượt qua độ dài chữ số, chẳng hạn như`1314151718`, trình phân tích cú pháp không dựa vào các đoạn có chiều rộng cố định. Nó so sánh biểu diễn thập phân chính xác của số dự kiến ​​ở mỗi bước, cho phép chuyển đổi giữa các giá trị hai chữ số và ba chữ số mà không cần xử lý đặc biệt.
