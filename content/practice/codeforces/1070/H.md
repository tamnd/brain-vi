---
title: "CF 1070H - Đề xuất tệp BerOS"
description: "Chúng tôi được cung cấp một tập hợp cố định các tên tệp ngắn và một luồng truy vấn. Mỗi truy vấn là một chuỗi ngắn và chúng ta phải xác định có bao nhiêu tên tệp chứa chuỗi đó dưới dạng chuỗi con liền kề."
date: "2026-06-15T13:55:49+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1070
codeforces_index: "H"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 1500
weight: 1070
solve_time_s: 379
verified: false
draft: false
---

[CF 1070H - Đề xuất tệp BerOS](https://codeforces.com/problemset/problem/1070/H) 

**Đánh giá:** 1500 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 6 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp cố định các tên tệp ngắn và một luồng truy vấn. Mỗi truy vấn là một chuỗi ngắn và chúng ta phải xác định có bao nhiêu tên tệp chứa chuỗi đó dưới dạng chuỗi con liền kề. Cùng với số lượng, chúng tôi cũng cần trả về bất kỳ tên tệp nào chứa chuỗi truy vấn hoặc dấu gạch ngang nếu không có tệp nào khớp. 

Một chi tiết cấu trúc quan trọng là tên tệp cực kỳ ngắn, tối đa 8 ký tự. Điều đó có nghĩa là mỗi tệp chỉ chứa một số lượng nhỏ chuỗi con và mọi chuỗi con có thể có cũng đều ngắn. Điều này gợi ý rõ ràng rằng chúng ta có đủ khả năng tính toán trước thông tin chuỗi con cho tất cả các tệp mà không phải lo lắng về việc hết bộ nhớ hoặc thời gian. 

Kích thước đầu vào cũng củng cố hướng này. Có tới 10.000 tệp và lên tới 50.000 truy vấn. Việc quét mỗi truy vấn trên tất cả các tệp sẽ quá chậm vì điều đó sẽ dẫn đến 500 triệu lần kiểm tra chuỗi con trong trường hợp xấu nhất. Mỗi lần kiểm tra đều rẻ, nhưng thang đo vẫn quá lớn dưới giới hạn 3 giây. Chúng tôi cần tiền xử lý để biến mỗi truy vấn thành tra cứu O(1) hoặc gần O(1). 

Trường hợp cạnh tinh tế là các chuỗi con lặp lại bên trong một tên tệp. Nếu một tập tin là`"aaaa"`, chuỗi con`"aa"`xuất hiện nhiều lần bên trong nó, nhưng nó vẫn chỉ đóng góp một lần vào số lượng. Một trường hợp đặc biệt khác là các truy vấn không tồn tại trong bất kỳ tệp nào, nơi chúng ta phải trả về`0 -`thay vì tên tệp trống hoặc mặc định rác. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là đơn giản. Đối với mỗi truy vấn, hãy lặp lại tất cả các tên tệp và kiểm tra xem chuỗi truy vấn có xuất hiện dưới dạng chuỗi con hay không. Vì mỗi tệp có độ dài tối đa là 8 nên việc kiểm tra chuỗi con diễn ra liên tục trong thực tế nhưng chúng tôi vẫn thực hiện việc này cho tất cả các tệp trên mỗi truy vấn. Điều này dẫn đến khoảng$O(n \cdot q \cdot L)$hành vi vượt xa giới hạn khi$n = 10^4$Và$q = 5 \cdot 10^4$. 

Điểm thất bại là sự lặp lại. Chúng tôi tính toán lại các mối quan hệ chuỗi con giống nhau nhiều lần cho mọi truy vấn, ngay cả khi tập hợp tệp là tĩnh. Cấu trúc gợi ý đảo ngược phối cảnh: thay vì hỏi từng truy vấn xem tệp nào chứa nó, chúng ta có thể tính toán trước mọi chuỗi con có thể chứa tệp đó. 

Mỗi tệp chỉ đóng góp một số lượng nhỏ chuỗi con vì độ dài tối đa là 8. Đối với một chuỗi có độ dài$L$, số chuỗi con là$L(L+1)/2$, nhiều nhất là 36. Trên 10.000 tệp, đây chỉ là khoảng 360.000 lần xuất hiện chuỗi con. Nó đủ nhỏ để lưu trữ trong bản đồ băm. 

Cải tiến quan trọng là xây dựng một từ điển ánh xạ mỗi chuỗi con thành hai phần thông tin: có bao nhiêu tệp riêng biệt chứa chuỗi đó và một tên tệp mẫu chứa chuỗi đó. Khi quá trình tiền xử lý này hoàn tất, mỗi truy vấn sẽ trở thành tra cứu trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot q \cdot L)$|$O(1)$| Quá chậm | 
| Tối ưu (chuỗi con tính toán trước) |$O(n \cdot L^2 + q)$|$O(n \cdot L^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý danh sách tệp một lần và xây dựng chỉ mục chung cho tất cả các chuỗi con. 

1. Đối với mỗi tên tệp, hãy tạo tất cả các chuỗi con riêng biệt. Chúng tôi sử dụng một bộ cho mỗi tệp để các ký tự lặp lại không gây ra việc đếm hai lần cùng một tệp cho một chuỗi con. Điều này đảm bảo tính chính xác khi cùng một chuỗi con xuất hiện nhiều lần trong một tệp. 
2. Đối với mỗi chuỗi con duy nhất của tệp, hãy cập nhật từ điển chung. Nếu chuỗi con chưa xuất hiện, hãy khởi tạo số đếm của nó về 0 và lưu trữ tệp hiện tại làm ví dụ. Sau đó tăng số lượng của nó lên một. Ví dụ được lưu trữ không cần cập nhật sau khi khởi tạo vì mọi tệp hợp lệ đều được chấp nhận. 
3. Sau khi xử lý trước tất cả các tệp, hãy xử lý từng truy vấn bằng cách kiểm tra xem nó có tồn tại trong từ điển hay không. Nếu có, hãy xuất số lượng được lưu trữ và tệp ví dụ. Nếu không, xuất ra số 0 và dấu gạch ngang. 

Lý do điều này hiệu quả là vì tất cả công việc tốn kém đều tập trung vào tiền xử lý và mỗi truy vấn trở thành tra cứu băm theo thời gian liên tục. 

### Tại sao nó hoạt động 

Mỗi chuỗi con được liên kết với chính xác tập hợp các tệp chứa chuỗi đó và chúng tôi ghi lại tư cách thành viên chính xác một lần cho mỗi tệp bằng cách sử dụng bộ chống trùng lặp cho mỗi tệp. Do đó, bộ đếm toàn cục biểu thị số lượng tệp riêng biệt chứa chuỗi con. Vì các truy vấn chỉ đọc từ ánh xạ được tính toán trước này nên không có truy vấn nào phụ thuộc vào quá trình quét thời gian chạy hoặc tính toán lại một phần, do đó các câu trả lời nhất quán với định nghĩa về sự xuất hiện chuỗi con trên toàn bộ tập hợp tệp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def all_substrings(s: str):
    res = set()
    n = len(s)
    for i in range(n):
        cur = []
        for j in range(i, n):
            cur.append(s[j])
            res.add("".join(cur))
    return res

def main():
    n = int(input())
    files = [input().strip() for _ in range(n)]

    mp = {}

    for name in files:
        subs = all_substrings(name)
        for sub in subs:
            if sub not in mp:
                mp[sub] = [0, name]
            mp[sub][0] += 1

    q = int(input())
    out = []

    for _ in range(q):
        s = input().strip()
        if s in mp:
            out.append(f"{mp[s][0]} {mp[s][1]}")
        else:
            out.append("0 -")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```chức năng`all_substrings`xây dựng tất cả các chuỗi con riêng biệt của tên tệp bằng cách sử dụng một bộ. Điều này tránh các trường hợp đếm quá mức như các ký tự lặp lại trong đó cùng một chuỗi con có thể xuất hiện nhiều lần trong một tệp. 

Từ điển`mp`lưu trữ cho mỗi chuỗi con một cặp bao gồm tần số của nó trên các tệp và một tên tệp đại diện. Người đại diện được cố định ngay lần gặp đầu tiên, vì bất kỳ câu trả lời hợp lệ nào đều được chấp nhận. 

Mỗi truy vấn được giải quyết bằng một lần tra cứu từ điển duy nhất, đảm bảo phản hồi ngay lập tức. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Các tập tin được`"test"`,`"contests"`,`"test."`, Và`".test"`. 

Đối với truy vấn`"ts"`, chúng tôi kiểm tra xem tập tin nào chứa nó. Cả hai`"contests"`Và`"test."`bao gồm`"ts"`, do đó số lượng là 2 và một trong hai tệp có thể được trả về. 

| Truy vấn | Tra cứu kết quả | Đếm | Tệp ví dụ | 
| --- | --- | --- | --- | 
| ts | tìm thấy | 2 | cuộc thi | 
| . | tìm thấy | 2 | .test | 
| tranh luận. | tìm thấy | 1 | Bài kiểm tra. | 
| st | tìm thấy | 4 | kiểm tra | 

Dấu vết này cho thấy rằng sau khi quá trình tiền xử lý hoàn tất, mỗi truy vấn sẽ giảm xuống quyền truy cập bảng băm trực tiếp. 

Bây giờ hãy xem xét một trường hợp tùy chỉnh nhỏ. 

Tập tin:`"aaa"`,`"aba"`Truy vấn:`"aa"`| Tập tin | Các chuỗi con đóng góp | 
| --- | --- | 
| aaa | aaa, aaa | 
| aba | ab, ba, aba | 

Sau khi tổng hợp,`"aa"`bản đồ để đếm 1 với ví dụ`"aaa"`. 

Điều này xác nhận rằng các lần xuất hiện lặp lại trong một tệp không làm tăng số lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot L^2 + q)$| Mỗi tệp đóng góp tối đa 36 chuỗi con, mỗi truy vấn là tra cứu O(1) | 
| Không gian |$O(n \cdot L^2)$| Tất cả các chuỗi con riêng biệt trên các tệp được lưu trữ trong bản đồ băm | 

Với$n \le 10^4$Và$L \le 8$, quá trình tiền xử lý tạo ra tối đa khoảng 360.000 chuỗi con, có thể dễ dàng quản lý được. Thời gian truy vấn chỉ tuyến tính đối với số lượng truy vấn, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def all_substrings(s: str):
        res = set()
        n = len(s)
        for i in range(n):
            cur = []
            for j in range(i, n):
                cur.append(s[j])
                res.add("".join(cur))
        return res

    n = int(input())
    files = [input().strip() for _ in range(n)]

    mp = {}
    for name in files:
        subs = all_substrings(name)
        for sub in subs:
            if sub not in mp:
                mp[sub] = [0, name]
            mp[sub][0] += 1

    q = int(input())
    out = []
    for _ in range(q):
        s = input().strip()
        if s in mp:
            out.append(f"{mp[s][0]} {mp[s][1]}")
        else:
            out.append("0 -")
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""4
test
contests
test.
.test
6
ts
.
st.
.test
contes.
st
""") == """1 contests
2 .test
1 test.
1 .test
0 -
4 test."""

# single character files
assert run("""2
a
b
3
a
b
c
""") == """1 a
1 b
0 -"""

# repeated characters inside file
assert run("""1
aaaa
3
a
aa
aaa
""") == """1 a
1 a
1 a"""

# no matches
assert run("""3
abc
def
ghi
2
zzz
xy
""") == """0 -
0 -"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tập tin ký tự đơn | trận đấu trực tiếp | tính đúng đắn cơ bản | 
| ký tự lặp đi lặp lại | không tính gấp đôi | chống trùng lặp trên mỗi tập tin | 
| không có trận đấu |`0 -`xử lý | trường hợp chuỗi con bị thiếu | 

## Vỏ cạnh 

Một tập tin như`"aaaa"`là trường hợp bẫy chính. Một cách tiếp cận bất cẩn đếm trực tiếp các lần xuất hiện chuỗi con sẽ được tính`"aa"`nhiều lần trong cùng một tập tin. Hành vi đúng yêu cầu xử lý từng tệp như một người đóng góp duy nhất cho mỗi chuỗi con. 

Đối với đầu vào:```
1
aaaa
1
aa
```Bước tiền xử lý tạo ra chuỗi con`{a, aa, aaa, aaaa}`. Mặc dù`"aa"`xuất hiện ở nhiều vị trí, nó chỉ được chèn một lần vào tập hợp mỗi tệp, do đó tổng số tổng thể trở thành 1. Truy vấn xuất ra chính xác`1 aaaa`. 

Trường hợp cạnh thứ hai là khi chuỗi con truy vấn không xuất hiện trong bất kỳ tệp nào. Đối với đầu vào:```
2
abc
def
1
z
```Tra cứu từ điển không thành công và kết quả là`0 -`. Điều này đảm bảo chúng tôi không cố truy cập vào tên tệp mẫu không xác định hoặc tạo ra giá trị cũ từ quá trình khởi tạo.
