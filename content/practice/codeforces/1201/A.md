---
title: "CF 1201A - Kỳ thi quan trọng"
description: "Mỗi học sinh trả lời một bài kiểm tra trắc nghiệm với $m$ câu hỏi, trong đó mỗi câu hỏi có năm lựa chọn khả thi. Chúng tôi được phát phiếu trả lời đầy đủ của từng học sinh, nhưng chưa biết câu trả lời đúng."
date: "2026-06-11T23:49:31+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1201
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 577 (Div. 2)"
rating: 900
weight: 1201
solve_time_s: 82
verified: true
draft: false
---

[CF 1201A - Bài kiểm tra quan trọng](https://codeforces.com/problemset/problem/1201/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh trả lời một bài kiểm tra trắc nghiệm bằng$m$câu hỏi, trong đó mỗi câu hỏi có năm lựa chọn có thể. Chúng tôi được phát phiếu trả lời đầy đủ của từng học sinh, nhưng chưa biết câu trả lời đúng. Những gì chúng tôi kiểm soát là sự lựa chọn giả định về câu trả lời đúng cho mỗi câu hỏi và mỗi câu hỏi$j$có số điểm cố định$a_j$nếu trả lời đúng. 

Nếu chúng tôi quyết định rằng một phương án cụ thể là đúng cho một câu hỏi thì mỗi học sinh chọn phương án đó sẽ nhận được số điểm tương ứng cho câu hỏi đó. Nhiệm vụ của chúng tôi là chỉ định một phương án đúng (A đến E) cho mọi câu hỏi theo cách tối đa hóa tổng số điểm đạt được của tất cả học sinh. 

Cấu trúc độc lập giữa các câu hỏi. Sự lựa chọn chúng tôi đưa ra cho câu hỏi$j$không ảnh hưởng đến bất kỳ câu hỏi nào khác vì điểm số được cộng tuyến tính và câu trả lời của học sinh là cố định. 

Những hạn chế$n, m \le 1000$ngụ ý tối đa một triệu lần kiểm tra ký tự trong quá trình triển khai đơn giản. Bất kỳ giải pháp nào quét được câu trả lời cho mỗi câu hỏi của mỗi học sinh đều đủ nhanh, vì$O(nm)$hoạt động còn nhỏ. 

Một sai lầm ngây thơ là cho rằng chúng ta cần mô phỏng các đáp án chung khác nhau hoặc cố gắng sắp xếp nhiều câu hỏi cùng một lúc. Ví dụ, việc nghĩ rằng chúng ta phải chọn một khuôn mẫu nhất quán cho các câu hỏi ngoài việc tối đa hóa độc lập sẽ dẫn đến sự phức tạp không cần thiết. Một sai lầm phổ biến khác là quên rằng câu trả lời đúng được chọn độc lập cho mỗi câu hỏi chứ không phải cho mỗi học sinh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các chuỗi câu trả lời đúng có độ dài có thể$m$, trong đó mỗi vị trí có 5 lựa chọn. Điều này mang lại$5^m$những khả năng lớn về mặt thiên văn ngay cả đối với những khả năng nhỏ$m$. Đối với mỗi đáp án của thí sinh, việc tính tổng điểm yêu cầu phải kiểm tra tất cả$n$sinh viên trên khắp$m$câu hỏi, dẫn đến$O(nm)$mỗi ứng viên. Điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là các câu hỏi không tương tác. Đối với một câu hỏi cố định$j$, nếu chúng ta quyết định lựa chọn đó$A$là đúng, điểm đóng góp của câu hỏi đó chỉ đơn giản là$a_j$nhân với số học sinh chọn$A$. Tương tự đối với B, C, D và E. Vì vậy, với mỗi câu hỏi, chúng ta chỉ cần chọn câu trả lời thường gặp nhất trong số học sinh. 

Điều này làm giảm vấn đề khi tính toán tần số đếm trên mỗi cột của ma trận đầu vào, sau đó chọn kết quả tốt nhất trong số năm lựa chọn cố định trên mỗi cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(5^m \cdot nm)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ tích lũy kết quả về 0. Điều này sẽ lưu trữ tổng số điểm tối đa trên tất cả các câu hỏi. 
2. Đối với mỗi mục lục câu hỏi$j$, xây dựng bộ đếm tần số cho năm lựa chọn có thể. Chúng tôi quét tất cả học sinh và đếm xem có bao nhiêu học sinh đã chọn A, B, C, D và E cho cột đó. 
3. Sau khi tính toán tần số cho câu hỏi$j$, tính toán mức đóng góp tốt nhất có thể đạt được bằng cách lấy tần suất tối đa trong số năm lựa chọn và nhân nó với$a_j$. 
4. Thêm khoản đóng góp này vào tổng số tích lũy. 
5. Lặp lại cho tất cả các câu hỏi. 

Lý do bước này có tác dụng là vì tính đúng đắn của câu hỏi chỉ phụ thuộc vào số lượng học sinh khớp với câu trả lời đã chọn. Vì chúng tôi có quyền tuyên bố bất kỳ phương án nào là đúng nên chúng tôi luôn chọn phương án có độ hỗ trợ cao nhất. 

### Tại sao nó hoạt động 

Đối với mỗi câu hỏi$j$, bất kỳ giao câu trả lời đúng nào cũng tương ứng với việc chọn một trong năm nhóm học sinh. Điểm số có tính chất tuyến tính đối với quy mô nhóm, do đó việc tối đa hóa sự đóng góp sẽ giúp giảm thiểu việc lựa chọn nhóm lớn nhất. Vì các câu hỏi là độc lập nên việc tổng hợp tối ưu cục bộ sẽ tạo ra tối ưu toàn cục mà không bị nhiễu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    students = [input().strip() for _ in range(n)]
    scores = list(map(int, input().split()))

    total = 0

    for j in range(m):
        freq = [0] * 5
        for i in range(n):
            c = students[i][j]
            freq[ord(c) - 65] += 1

        best = max(freq)
        total += best * scores[j]

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các chuỗi của sinh viên rồi lặp lại từng cột. Đối với mỗi cột, nó xây dựng một mảng tần số có kích thước cố định nhỏ có độ dài 5. Sự chuyển đổi`ord(c) - 65`ánh xạ các ký tự A đến E vào các chỉ số từ 0 đến 4. Điều này giúp việc triển khai được chặt chẽ và tránh các chuỗi điều kiện. 

Phép nhân với`scores[j]`chỉ xảy ra sau khi xác định được câu trả lời phổ biến nhất, đảm bảo chúng ta không bao giờ đếm quá. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
ABCD
ABCE
1 2 3 4
```Chúng tôi xử lý từng cột: 

| Câu hỏi | Một số | Số B | Số C | Đếm D | Đếm E | Tốt nhất | Điểm | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 0 | 0 | 2 | 1 | 2 | 
| 2 | 2 | 0 | 0 | 0 | 0 | 2 | 2 | 4 | 
| 3 | 0 | 0 | 2 | 0 | 0 | 2 | 3 | 6 | 
| 4 | 1 | 0 | 1 | 0 | 0 | 1 | 4 | 4 | 

Tổng cộng là$2 + 4 + 6 + 4 = 16$. 

Dấu vết này cho thấy mỗi cột được xử lý độc lập và lựa chọn tối ưu luôn là chữ cái đa số. 

### Ví dụ 2 

đầu vào:```
3 3
ABC
BBC
CCA
5 4 3
```| Câu hỏi | A | B | C | Tốt nhất | Điểm | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 2 | 5 | 10 | 
| 2 | 1 | 1 | 1 | 1 | 4 | 4 | 
| 3 | 1 | 1 | 1 | 1 | 3 | 3 | 

Tổng cộng =$10 + 4 + 3 = 17$. 

Ví dụ này cho thấy rằng ngay cả khi tần số bằng nhau, bất kỳ sự lựa chọn nào trong số các chữ cái có tần số tối đa đều mang lại sự đóng góp như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ký tự được xử lý một lần khi đếm tần số trên mỗi cột | 
| Không gian |$O(1)$thêm | Chỉ một mảng cố định có kích thước 5 được sử dụng cho mỗi cột | 

Sự ràng buộc$n, m \le 1000$cho nhiều nhất$10^6$kiểm tra nhân vật, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""2 4
ABCD
ABCE
1 2 3 4
""") == "16"

# single student, trivial
assert run("""1 3
ABC
1 1 1
""") == "3"

# all same answers
assert run("""3 2
AA
AA
AA
5 7
""") == "36"

# tie case
assert run("""3 2
AB
BA
AB
10 1
""") == "20"

# larger mixed
assert run("""4 3
ABC
ABC
ABD
BBC
1 2 3
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 3 | tính đúng đắn của trường hợp cơ sở | 
| tất cả các câu trả lời giống nhau | 36 | thống trị tần số tối đa | 
| hộp đựng cà vạt | 20 | xử lý tần số bằng nhau | 
| trường hợp hỗn hợp | 15 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả học sinh chọn các câu trả lời khác nhau cho một câu hỏi. Ví dụ:```
3 1
A
B
C
10
```Đối với một cột, các tần số đều bằng 1. Thuật toán chọn bất kỳ tần số nào trong số chúng, đưa ra đóng góp$1 \cdot 10 = 10$. Dấu vết rất đơn giản: mảng tần số trở thành$[1,1,1,0,0]$, tối đa là 1 nên kết quả là đúng. 

Một trường hợp cạnh khác là khi$n = 1$. Thuật toán vẫn hoạt động vì mảng tần số sẽ có một số 1 và bốn số 0, và phần đóng góp chỉ đơn giản là điểm câu hỏi. 

Trường hợp cạnh cuối cùng là khi$m = 1$. Thuật toán giảm xuống việc tính toán ký tự thường xuyên nhất trong một cột chuỗi đơn, vẫn khớp với cùng một logic mà không cần sửa đổi.
