---
title: "CF 106145F - Tung Tung Tung"
description: "Bài toán cho hai dây chỉ được tạo từ L và R. Dây đầu tiên mô tả chuỗi đánh trống thực tế. Mỗi lần nhấn sẽ tạo ra một ký tự hiển thị hoặc hai ký tự hiển thị giống hệt nhau trong âm thanh được ghi. Chuỗi thứ hai là đoạn ghi âm mà chúng ta nghe được."
date: "2026-06-25T11:29:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106145
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-29-25"
rating: 0
weight: 106145
solve_time_s: 30
verified: true
draft: false
---

[CF 106145F - Tung Tung Tung](https://codeforces.com/problemset/problem/106145/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra hai chuỗi chỉ được tạo từ`L`Và`R`. Chuỗi đầu tiên mô tả chuỗi đánh trống thực tế. Mỗi lần nhấn sẽ tạo ra một ký tự hiển thị hoặc hai ký tự hiển thị giống hệt nhau trong âm thanh được ghi. Chuỗi thứ hai là đoạn ghi âm mà chúng ta nghe được. Chúng ta cần quyết định xem mỗi lần truy cập trong chuỗi đầu tiên có thể được mở rộng thành một hoặc hai bản sao của cùng một ký tự để tạo thành chuỗi thứ hai hay không. 

Ví dụ: một chuỗi hit`LR`có thể trở thành`LR`,`LLR`,`LRR`, hoặc`LLRR`. Việc mở rộng mỗi ký tự gốc là độc lập, vì vậy thay đổi duy nhất được phép là mỗi ký tự có thể được sao chép một lần. 

Các ràng buộc này làm cho việc mô phỏng trực tiếp mọi khả năng mở rộng là không thể. Nếu một chuỗi có độ dài`n`, mỗi ký tự có hai lựa chọn, do đó, việc thử tất cả các bản ghi có thể sẽ yêu cầu kiểm tra tối đa`2^n`khả năng. Ngay cả đối với vài chục nhân vật, điều này cũng không thể thực hiện được. Giải pháp dự định phải xử lý mỗi ký tự với số lần không đổi, đưa ra`O(n)`thuật toán. 

Phần khó khăn không phải là những chuỗi lớn mà là ranh giới giữa các nhóm ký tự bằng nhau. Một lỗi phổ biến là chỉ kiểm tra xem hai chuỗi có dạng nén giống nhau hay không. Chỉ nén thôi là không đủ vì độ dài của các nhóm rất quan trọng. 

Ví dụ:```
Input:
p = "L"
s = "LLL"
```Đầu ra đúng là:```
NO
```Một giải pháp bất cẩn có thể thấy rằng cả hai chuỗi chỉ bao gồm`L`và chấp nhận chúng. Tuy nhiên, một lần truy cập ban đầu có thể tạo ra tối đa hai lần`L`ký tự, vì vậy ba ký tự là không thể. 

Một trường hợp đặc biệt khác là khi thứ tự của các nhóm thay đổi:```
Input:
p = "LR"
s = "LLL"
```Đầu ra đúng là:```
NO
```Chuỗi thứ hai chỉ có một nhóm`L`, trong khi chuỗi ban đầu có hai lần truy cập khác nhau. Sự mất tích`R`không thể được tạo ra bằng cách mở rộng một`L`. 

Trường hợp ranh giới cuối cùng là khi chuỗi được ghi ngắn hơn:```
Input:
p = "LL"
s = "L"
```Đầu ra đúng là:```
NO
```Mỗi ký tự gốc phải đóng góp ít nhất một ký tự, vì vậy hai lần truy cập không thể biến thành một ký tự được ghi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để mở rộng chuỗi đầu tiên. Đối với mỗi ký tự, chúng tôi chọn xem nó ở dạng một ký tự hay trở thành hai bản sao, sau đó so sánh kết quả được tạo với bản ghi. Điều này đúng vì nó xem xét mọi cách giải thích có thể có của các lượt truy cập. Vấn đề là một chuỗi có độ dài`n`có`2^n`khả năng mở rộng nhanh chóng trở nên quá lớn. 

Quan sát hữu ích là mỗi ký tự trong chuỗi gốc tương ứng với một khối liên tiếp của cùng một ký tự trong bản ghi. MỘT`L`chỉ có thể tạo`L`các ký tự và nó phải tạo một hoặc hai trong số chúng. Điều tương tự cũng áp dụng cho`R`. 

Điều này có nghĩa là chúng ta không cần xây dựng các chuỗi mở rộng có thể có. Chúng ta chỉ cần duyệt qua cả hai chuỗi và so sánh các nhóm phù hợp. Nếu chuỗi gốc có một nhóm`k`các ký tự giống nhau, chuỗi được ghi phải có một nhóm ký tự giống nhau có độ dài từ`k`Và`2k`. Trong vấn đề này, vì mỗi ký tự của chuỗi gốc là một lần truy cập riêng biệt nên độ dài của mỗi nhóm ban đầu được tính trực tiếp. Điều này làm giảm vấn đề so sánh độ dài chạy. 

Lực lượng vũ phu hoạt động vì mọi bản mở rộng hợp lệ đều tuân theo các quy tắc của lượt truy cập, nhưng không thành công vì nó khám phá các lựa chọn có thể được kiểm tra cục bộ. Quan sát cho thấy mỗi lần truy cập chỉ ảnh hưởng đến khối liên tiếp của chính nó cho phép chúng tôi thay thế việc khám phá theo cấp số nhân bằng quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt hai con trỏ ở đầu chuỗi gốc và chuỗi đã ghi. Các con trỏ biểu thị lần nhấn và âm thanh chưa được xử lý tiếp theo. 
2. Đối với ký tự hiện tại trong chuỗi gốc, hãy đếm xem nó có bao nhiêu ký tự bằng nhau liên tiếp. Thực hiện tương tự cho nhóm tương ứng trong chuỗi đã ghi. Các nhóm phải bắt đầu với cùng một ký tự vì đòn đánh không thể thay đổi từ`L`ĐẾN`R`hoặc ngược lại. 
3. Kiểm tra mối quan hệ giữa độ dài hai nhóm. Độ dài nhóm được ghi ít nhất phải bằng độ dài nhóm ban đầu vì mỗi lần đánh sẽ tạo ra một âm thanh và tối đa phải gấp đôi độ dài nhóm ban đầu vì mỗi lần đánh có thể tạo ra tối đa hai âm thanh. 
4. Di chuyển cả hai con trỏ tới các nhóm tiếp theo và lặp lại cho đến khi hết một trong các chuỗi. 
5. Sau khi quá trình quét kết thúc, chỉ chấp nhận nếu cả hai chuỗi đã được sử dụng hết. Bất kỳ ký tự nào còn lại có nghĩa là một chuỗi chứa các lượt truy cập hoặc âm thanh chưa từng có. 

Tại sao nó hoạt động: 

Mỗi khối tối đa gồm các ký tự bằng nhau trong chuỗi gốc biểu thị một chuỗi các lần truy cập liên tiếp trên cùng một trống. Những lần truy cập đó không thể trộn lẫn với các ký tự khác trong bản ghi, vì vậy chúng phải tương ứng với chính xác một khối trong chuỗi được ghi. Sự khác biệt duy nhất có thể xảy ra là độ dài của khối đó, vì mỗi lần truy cập đóng góp một hoặc hai ký tự. Thuật toán kiểm tra chính xác điều kiện này cho từng khối, do đó nó chấp nhận chính xác các bản ghi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        p = input().strip()
        s = input().strip()

        i = 0
        j = 0
        ok = True

        while i < len(p) and j < len(s):
            if p[i] != s[j]:
                ok = False
                break

            pi = i
            while i < len(p) and p[i] == p[pi]:
                i += 1

            sj = j
            while j < len(s) and s[j] == s[sj]:
                j += 1

            p_len = i - pi
            s_len = j - sj

            if s_len < p_len or s_len > 2 * p_len:
                ok = False
                break

        if i != len(p) or j != len(s):
            ok = False

        ans.append("YES" if ok else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp chính giữ hai chỉ số, một chỉ số cho mỗi chuỗi. Nó không bao giờ di chuyển ngược lại, đó là lý do tại sao tổng công là tuyến tính. 

Các vòng lặp bên trong tìm điểm cuối của khối hiện tại. Sau khi họ làm xong,`p_len`là số lần truy cập tạo ra khối này và`s_len`là số lượng âm thanh trong bản ghi âm. Phạm vi hợp lệ duy nhất là từ`p_len`ĐẾN`2 * p_len`. 

Sự so sánh cuối cùng của`i`Và`j`là cần thiết vì vòng lặp chính có thể dừng sau khi một chuỗi kết thúc. Nếu không có kiểm tra này, việc triển khai có thể chấp nhận kết quả khớp tiền tố không chính xác trong khi bỏ qua các ký tự phụ. 

Số nguyên Python không phải là vấn đề đáng lo ngại ở đây vì thuật toán chỉ lưu trữ độ dài và chỉ số. Các giá trị lớn nhất được giới hạn bởi kích thước đầu vào. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
p = "LR"
s = "LLRR"
```| Bước | Nhóm gốc | Nhóm ghi âm | Chiều dài ban đầu | Chiều dài ghi lại | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | L | LL | 1 | 2 | hợp lệ | 
| 2 | R | RR | 1 | 2 | hợp lệ | 

Cả hai nhóm đều thỏa mãn quy tắc một đến hai bản sao, vì vậy câu trả lời là`YES`. 

Ví dụ này cho thấy mọi lần truy cập có thể bị trùng lặp và các nhóm vẫn phải xuất hiện theo cùng một thứ tự. 

Coi như:```
p = "LR"
s = "LLLR"
```| Bước | Nhóm gốc | Nhóm ghi âm | Chiều dài ban đầu | Chiều dài ghi lại | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | L | LLL | 1 | 3 | không hợp lệ | 

Cú đánh đầu tiên sẽ cần tạo ra ba`L`nhân vật, điều đó là không thể. Thuật toán từ chối ngay lập tức. 

Ví dụ này chứng tỏ tại sao chỉ kiểm tra thứ tự ký tự là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự trong cả hai chuỗi đều được truy cập một lần. | 
| Không gian | O(1) | Chỉ con trỏ, bộ đếm và trạng thái câu trả lời được lưu trữ. | 

Tổng chiều dài của tất cả các chuỗi được xử lý thông qua một lần chuyển. Điều này dễ dàng phù hợp với giới hạn lập trình cạnh tranh điển hình vì thuật toán chỉ thực hiện công việc không đổi trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        p = input().strip()
        s = input().strip()

        i = 0
        j = 0
        ok = True

        while i < len(p) and j < len(s):
            if p[i] != s[j]:
                ok = False
                break

            a = i
            while i < len(p) and p[i] == p[a]:
                i += 1

            b = j
            while j < len(s) and s[j] == s[b]:
                j += 1

            x = i - a
            y = j - b

            if y < x or y > 2 * x:
                ok = False
                break

        if i != len(p) or j != len(s):
            ok = False

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

assert solution("""5
R
RR
LRLR
LRLR
LR
LLLR
LLLLLRL
LLLLRRLL
LLRLRLRRL
LLLRLRRLLRRRL
""") == """YES
YES
NO
NO
YES"""

assert solution("""1
L
L
""") == "YES"

assert solution("""1
L
LLL
""") == "NO"

assert solution("""1
LR
LLRR
""") == "YES"

assert solution("""1
LL
L
""") == "NO"

assert solution("""1
LLLL
LLLLLLLL
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`L`Và`L`|`YES`| Trường hợp hợp lệ tối thiểu | 
|`L`Và`LLL`|`NO`| Ngăn chặn việc chấp nhận các nhóm quá dài | 
|`LR`Và`LLRR`|`YES`| Kiểm tra sự trùng lặp của nhiều nhóm | 
|`LL`Và`L`|`NO`| Ngăn chặn việc chấp nhận âm thanh bị thiếu | 
|`LLLL`Và`LLLLLLLL`|`YES`| Kiểm tra ranh giới nhóm lớn | 

## Vỏ cạnh 

Đối với trường hợp:```
p = "L"
s = "LLL"
```thuật toán so sánh các nhóm duy nhất. Độ dài nhóm ban đầu là`1`và độ dài nhóm được ghi lại là`3`. Từ`3`lớn hơn`2 * 1`, điều kiện không thành công và câu trả lời là`NO`. 

Đối với trường hợp:```
p = "LR"
s = "LLL"
```so sánh nhóm đầu tiên nhìn thấy`L`chống lại`LLL`. Nhóm được ghi có độ dài`3`, đã vượt quá mức mở rộng tối đa có thể có của một lần truy cập. Thuật toán dừng lại mà không cố gắng khớp phần còn lại`R`, trở lại chính xác`NO`. 

Đối với trường hợp:```
p = "LL"
s = "L"
```nhóm ban đầu yêu cầu hai âm thanh vì có hai bản hit. Nhóm được ghi chỉ chứa một ký tự nên độ dài của nó nhỏ hơn độ dài yêu cầu tối thiểu. Thuật toán từ chối đầu vào. 

Đối với trường hợp:```
p = "LLLL"
s = "LLLLLLLL"
```thuật toán coi toàn bộ chuỗi là một nhóm. Độ dài nhóm ban đầu là`4`và độ dài nhóm được ghi lại là`8`, đó chính xác là giá trị lớn nhất được phép. Đầu vào được chấp nhận.
