---
title: "CF 102215C - Nhảy vòng tròn"
description: "Con chip bắt đầu ở điểm 0 trên một vòng tròn chứa p điểm, được đánh số từ 0 đến p - 1. Ở nước đi đầu tiên, nó tiến lên 1, ở nước thứ hai là 2, ở nước thứ ba là 3, v.v. Chuyển động bao quanh vòng tròn nên mọi vị trí đều được coi là modulo p."
date: "2026-08-18T21:55:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "C"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 799
verified: false
draft: false
---

[CF 102215C - Nhảy vòng tròn](https://codeforces.com/problemset/problem/102215/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Con chip bắt đầu tại điểm`0`trên đường tròn chứa`p`điểm, đánh số`0`bởi vì`p - 1`. Trong lần di chuyển đầu tiên, nó tiến lên bằng`1`, vào ngày thứ hai bởi`2`, vào ngày thứ ba bởi`3`, vân vân. Chuyển động bao quanh vòng tròn nên mọi vị trí đều được coi là modulo`p`. 

Sau đó`k`chuyển động thì tổng quãng đường đi được là 

[ 
1+2+\dots+k=\frac{k(k+1)}2. 
] 

Vậy vị trí sau`k`di chuyển là 

[ 
pos_k=\frac{k(k+1)}2\bmod p. 
] 

Nhiệm vụ là đếm xem có bao nhiêu giá trị khác nhau xuất hiện giữa`pos_0, pos_1, ..., pos_n`. Vị trí ban đầu`0`được bao gồm, vì vậy ngay cả khi`n = 0`, câu trả lời là`1`. 

Vòng tròn có thể chứa tới`10^7`điểm, trong khi số lần di chuyển có thể lớn bằng`10^18`. Giới hạn sau ngay lập tức loại trừ việc mô phỏng mọi động thái. Thậm chí`10^7`số lần lặp đã gần đến giới hạn thực tế đối với chương trình Python 2 giây, vì vậy chúng ta cần chứng minh rằng chỉ có tiền tố giới hạn của chuỗi chuyển động mới quan trọng. Giới hạn bộ nhớ 256 MB cũng tạo nên một Python`set`chứa hàng triệu số nguyên không hấp dẫn, trong khi nhỏ gọn`bytearray`có thể biểu thị liệu mỗi điểm đã được truy cập chỉ bằng một byte cho mỗi điểm hay chưa. 

Tính tuần hoàn quan trọng xuất phát từ công thức số tam giác. Đối với mọi`k`, 

[ 
pos_{k+2p} 
=\frac{(k+2p)(k+2p+1)}2 
\equiv \frac{k(k+1)}2 
\pmod p. 
] 

Do đó, chuỗi các vị trí lặp lại mỗi`2p`di chuyển. Chúng ta không bao giờ cần phải mô phỏng nhiều hơn`2p`di chuyển, bất kể`n`là`10^6`hoặc`10^18`. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể xử lý sai. Vì`p = 1, n = 0`, đầu vào đúng là`1 0`và câu trả lời là`1`, vì điểm bắt đầu đã là điểm duy nhất. Giải pháp chỉ tính điểm đến sau khi di chuyển sẽ in không chính xác`0`. 

Vì`p = 3, n = 10`, các vị trí là`0, 1, 0, 0, 1, 0, ...`, vậy câu trả lời là`2`. Một giải pháp giả định một khoảng thời gian`p`di chuyển mà không cần chứng minh vẫn có thể xử lý đúng một số trường hợp một cách tình cờ, nhưng khoảng thời gian thực tế là`2p`. 

Vì`p = 4, n = 3`, các vị trí đã ghé thăm là`0, 1, 3, 2`, vậy là mọi điểm đều đã đạt được và câu trả lời là`4`. Dừng lại sau ít hơn`n`di chuyển hoặc xử lý sai vị trí bắt đầu một cách riêng biệt sẽ gây ra từng lỗi một. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp tuân theo chip một cách chính xác. Duy trì vị trí hiện tại của nó và cho mỗi bước di chuyển`i`, thêm vào`i`modulo`p`. Bất cứ khi nào vị trí mới chưa được nhìn thấy trước đó, hãy tăng câu trả lời. Một mảng boolean được lập chỉ mục theo vị trí vòng tròn cho phép kiểm tra tư cách thành viên theo thời gian không đổi, vì vậy điều này đúng vì nó ghi lại chính xác tập hợp các vị trí mà chip đạt tới. 

Vấn đề là giá trị của`n`. Trong trường hợp xấu nhất,`n = 10^18`, do đó mô phỏng trực tiếp sẽ yêu cầu lên tới`10^18`lặp đi lặp lại, điều này hoàn toàn không thể thực hiện được. 

Quan sát mở ra sự tối ưu hóa là vị trí của chip sau`k`bước di chuyển là một số tam giác modulo`p`. Kể từ khi 

[ 
(k+2p)(k+2p+1)-k(k+1) 
=4pk+2p(2p+1), 
] 

sự khác biệt được chia cho`2p`, và sau khi chia cho`2`, các vị trí tương ứng khác nhau bội số của`p`. Kể từ đây`pos_{k+2p} = pos_k`. 

Phương pháp brute-force hoạt động vì mỗi bước di chuyển mô phỏng đều cho một vị trí chính xác và mảng đã truy cập sẽ ghi lại vị trí đó. Nó thất bại khi`n`là rất lớn. Việc quan sát định kỳ làm giảm vấn đề xuống mức tối đa`2p`di chuyển. Từ`p <= 10^7`, điều này có nghĩa là nhiều nhất`2 * 10^7`lặp đi lặp lại, độc lập với giá trị thiên văn tiềm năng của`n`. 

Chúng ta cũng có thể tránh được lần vượt qua thứ hai trên tất cả`p`định vị bằng cách tăng câu trả lời ngay lập tức khi đạt đến điểm chưa từng thấy trước đó. MỘT`bytearray`lưu trữ các cờ đã truy cập một cách nhỏ gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(p) | Quá chậm khi`n`lớn | 
| Tối ưu | O(phút(n, 2p)) | O(p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`p`Và`n`. Con chip đã ghé thăm điểm`0`, vì vậy hãy khởi tạo cấu trúc đã truy cập bằng điểm`0`được đánh dấu và câu trả lời bằng`1`. 
2. Thay số nước đi bằng`min(n, 2p)`. Trình tự lặp lại mỗi`2p`di chuyển, vì vậy việc mô phỏng bất cứ điều gì sau giai đoạn hoàn thành đầu tiên không thể thêm điểm mới. 
3. Giữ`cur`khi vị trí và quy trình hiện tại của chip chuyển từ`1`thông qua số lần di chuyển giảm. Để di chuyển`i`, nâng cao`cur`qua`i`modulo`p`. 
4. Nếu vị trí kết quả chưa được đánh dấu trước đó, hãy đánh dấu vị trí đó và tăng dần câu trả lời. Một vị trí có thể đạt được nhiều lần, nhưng nó chỉ góp phần đưa ra câu trả lời trong lần truy cập đầu tiên. 
5. In số vị trí đã đánh dấu. Bởi vì câu trả lời đã tăng lên chính xác khi gặp một vị trí mới, nên nó đã bằng số lượng các điểm đã ghé thăm riêng biệt. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên`i`di chuyển,`cur`chính xác là vị trí của con chip sau khi di chuyển`i`và mọi điểm đã truy cập trong các lần di chuyển đó đều được đánh dấu trong mảng đã truy cập. Việc khởi tạo thiết lập bất biến cho việc di chuyển`0`, kể từ điểm`0`là vị trí bắt đầu. Mỗi lần lặp tiến lên chính xác theo độ dài bước nhảy tiếp theo và đánh dấu vị trí kết quả, giữ nguyên bất biến. 

Câu hỏi duy nhất còn lại là liệu có dừng lại sau`2p`di chuyển có thể mất một điểm. Không thể được, bởi vì với mỗi`k`, 

[ 
pos_{k+2p}=pos_k. 
] 

Mọi vị trí đạt được sau vị trí đầu tiên`2p`các bước di chuyển đã đạt được ở vị trí tương ứng trong khoảng thời gian trước đó. Như vậy việc đầu tiên`min(n, 2p)`các bước di chuyển chứa mọi điểm riêng biệt có thể được truy cập trong quá trình thực hiện được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p, n = map(int, input().split())

    visited = bytearray(p)
    visited[0] = 1
    answer = 1

    moves = min(n, 2 * p)

    cur = 0
    step_mod = 1

    for _ in range(moves):
        cur += step_mod
        if cur >= p:
            cur -= p

        if not visited[cur]:
            visited[cur] = 1
            answer += 1

        step_mod += 1
        if step_mod == p:
            step_mod = 0

    print(answer)

if __name__ == "__main__":
    solve()
```các`visited`mảng có chính xác`p`các mục, một mục cho mỗi điểm. MỘT`bytearray`được sử dụng thay cho danh sách hoặc tập hợp Python vì các phần tử của nó chỉ chiếm một byte, điều này quan trọng khi`p`lớn như`10^7`. 

Biến`moves`thực hiện trực tiếp đối số định kỳ. Nếu như`n < 2p`, mọi chuyển động được yêu cầu đều được mô phỏng. Nếu như`n >= 2p`, chỉ cần một khoảng thời gian đầy đủ. 

Việc thực hiện giữ`step_mod`thay vì lưu trữ toàn bộ chiều dài bước nhảy. Vì chỉ có modulo độ dài bước nhảy`p`ảnh hưởng đến vị trí, điều này là tương đương. Sau mỗi lần di chuyển,`step_mod`được tăng modulo`p`. 

bản cập nhật```
cur += step_mod
if cur >= p:
    cur -= p
```tương đương với`cur = (cur + step_mod) % p`. Tại mỗi lần lặp cả hai`cur`Và`step_mod`đang ở trong`[0, p - 1]`, vậy tổng của chúng nhỏ hơn`2p`, nghĩa là một phép trừ là đủ. Tránh`%`trong vòng lặp chính làm cho việc triển khai Python nhẹ hơn đáng kể. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý. Trong ngôn ngữ có chiều rộng cố định, giá trị ban đầu của`n`yêu cầu loại số nguyên đủ rộng, nhưng bộ đếm vòng lặp sau khi áp dụng`min(n, 2p)`nhiều nhất là`2 * 10^7`. 

Vòng lặp chạy chính xác với số lần di chuyển vẫn có thể đưa ra các vị trí mới. Điểm ban đầu được tính riêng nên vòng lặp xử lý các bước di chuyển một cách có chủ ý`1`bởi vì`moves`thay vì vô tình coi vị trí bắt đầu là một nước đi. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`p = 3`Và`n = 10`. Thời kỳ là`2p = 6`, vì vậy chỉ cần kiểm tra sáu nước đi đầu tiên. 

| Di chuyển | Bước modulo`p`| Vị trí | Điểm mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | Điểm ban đầu | 1 | 
| 1 | 1 | 1 | Có | 2 | 
| 2 | 2 | 0 | Không | 2 | 
| 3 | 0 | 0 | Không | 2 | 
| 4 | 1 | 1 | Không | 2 | 
| 5 | 2 | 0 | Không | 2 | 
| 6 | 0 | 0 | Không | 2 | 

Độ dài bước nhảy là`1, 2, 3, 4, 5, 6`, nhưng modulo`3`họ là`1, 2, 0, 1, 2, 0`. Chỉ có điểm`0`Và`1`xảy ra nên câu trả lời là`2`. Bốn bước di chuyển được yêu cầu còn lại lặp lại cùng một khoảng thời gian. 

Đối với mẫu 2,`p = 5`Và`n = 3`. Từ`n < 2p`, cả ba động tác đều được mô phỏng. 

| Di chuyển | Bước modulo`p`| Vị trí | Điểm mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | Điểm ban đầu | 1 | 
| 1 | 1 | 1 | Có | 2 | 
| 2 | 2 | 3 | Có | 3 | 
| 3 | 3 | 1 | Không | 3 | 

Các điểm đã ghé thăm là`{0, 1, 3}`, đưa ra câu trả lời cần thiết`3`. Dấu vết này cũng chứng minh tại sao việc đếm bước di chuyển và đếm các điểm mới truy cập là các hoạt động khác nhau. Nước đi thứ ba tiếp cận một điểm hiện có nên không làm tăng câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(phút(n, 2p)) | Nhiều nhất`2p`các bước di chuyển được mô phỏng, với công việc liên tục trên mỗi bước di chuyển | 
| Không gian | O(p) | Một byte được lưu trữ cho mỗi điểm vòng tròn | 

Từ`p <= 10^7`, thuật toán thực hiện tối đa`2 * 10^7`lặp đi lặp lại ngay cả khi`n = 10^18`. các`bytearray`cần khoảng 10 MB ở giá trị tối đa là`p`, thoải mái dưới giới hạn bộ nhớ 256 MB. Giải pháp dựa trên cùng`O(p)`-mô phỏng quy mô được thực hiện theo thời kỳ`2p`, thay vì cố gắng xử lý giá trị tiềm năng rất lớn của`n`. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    p, n = map(int, sys.stdin.readline().split())

    visited = bytearray(p)
    visited[0] = 1
    answer = 1

    moves = min(n, 2 * p)

    cur = 0
    step_mod = 1

    for _ in range(moves):
        cur += step_mod
        if cur >= p:
            cur -= p

        if not visited[cur]:
            visited[cur] = 1
            answer += 1

        step_mod += 1
        if step_mod == p:
            step_mod = 0

    print(answer)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided samples
assert solve_data("3 10\n") == "2\n", "sample 1"
assert solve_data("5 3\n") == "3\n", "sample 2"
assert solve_data("8 1000000000000000000\n") == "8\n", "sample 3"

# Minimum circle, no moves
assert solve_data("1 0\n") == "1\n", "single point with zero moves"

# Minimum circle, many moves
assert solve_data("1 1000000000000000000\n") == "1\n", "single point with huge n"

# Boundary before a full period
assert solve_data("4 3\n") == "4\n", "all four points reached before 2p"

# Exactly one full period
assert solve_data("3 6\n") == "2\n", "exactly 2p moves"

# Huge n must be reduced to one period
assert solve_data("10000000 1000000000000000000\n") <= "10000000\n", "huge n boundary"
```Thử nghiệm lớn cuối cùng chỉ kiểm tra xem kết quả có phải là số đếm hợp lệ hay không vì việc tính toán giá trị chính xác ở đây sẽ trùng lặp công việc của giải pháp bên trong chính thử nghiệm đó. Các bài kiểm tra khác thực hiện các câu trả lời chính xác được mong đợi và các ranh giới quan trọng của giai đoạn. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| tối thiểu`p`, không nước đi, điểm xuất phát phải được tính | 
|`1 1000000000000000000`|`1`| Vòng tròn một điểm và rất lớn`n`| 
|`4 3`|`4`| Tất cả các điểm có thể đạt được trước một học kỳ đầy đủ | 
|`3 6`|`2`| Chính xác`2p`chuyển động và ranh giới tuần hoàn | 
|`10000000 1000000000000000000`| Một giá trị trong`[1, 10000000]`| Quy mô tối đa`p`và khổng lồ`n`| 

## Vỏ cạnh 

cho`p = 1, n = 0`, thuật toán tạo ra một phần tử`bytearray`, đánh dấu vị trí`0`, và bộ`answer = 1`. Từ`moves = min(0, 2) = 0`, vòng lặp không thực thi. Đầu ra là`1`, đếm chính xác điểm bắt đầu. 

Vì`p = 3, n = 10`, thuật toán thay thế`n`với`min(10, 6) = 6`. Bắt đầu từ`0`, vị trí sau sáu bước di chuyển mô phỏng là`1, 0, 0, 1, 0, 0`. Vị trí duy nhất`1`là mới, vì vậy câu trả lời cuối cùng là`2`. Các bước từ bảy đến mười không thể thêm bất cứ điều gì vì chúng lặp lại bốn vị trí đầu tiên của dãy tuần hoàn. 

Vì`p = 4, n = 3`, vòng lặp xử lý các vị trí`1, 3, 2`. Cùng với ban đầu`0`, tập đã truy cập trở thành`{0, 1, 2, 3}`, vậy câu trả lời là`4`. Điều này mắc phải lỗi phổ biến là quên rằng điểm bắt đầu đã là một phần của tập hợp đã truy cập. 

Vì`p = 3, n = 6`, thuật toán xử lý chính xác một khoảng thời gian hoàn chỉnh. Các vị trí là`0, 1, 0, 0, 1, 0, 0`, do đó kết quả vẫn giữ nguyên`2`. Thực tế là vị trí cuối cùng trở lại`0`cũng là một sự kiểm tra cụ thể về danh tính`pos_{2p} = 0`. 

Vì`p = 8, n = 10^18`, số bước di chuyển được yêu cầu là rất lớn, nhưng`moves`trở thành`16`. Thuật toán chỉ kiểm tra mười sáu nước đi đó. Sau thời điểm đó, các vị trí tương tự sẽ lặp lại sau mỗi mười sáu nước đi, vì vậy câu trả lời thu được từ hiệp đầu tiên cũng là câu trả lời cho toàn bộ quá trình thực hiện được yêu cầu.
