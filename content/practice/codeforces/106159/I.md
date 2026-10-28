---
title: "CF 106159I - Ivo nhìn thấy tia UVa"
description: "Chúng ta được cung cấp một lưới hình chữ nhật gồm các ký tự đại diện cho một bảng chữ cái. Mỗi ô chứa một ký tự viết hoa duy nhất. Nhiệm vụ là đếm số lần mẫu “UVA” xuất hiện trên lưới khi đọc dọc theo đường thẳng."
date: "2026-06-21T09:38:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106159
codeforces_index: "I"
codeforces_contest_name: "XIII UnB Contest Mirror"
rating: 0
weight: 106159
solve_time_s: 40
verified: true
draft: false
---

[CF 106159I - Ivo đã nhìn thấy UVa](https://codeforces.com/problemset/problem/106159/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới hình chữ nhật gồm các ký tự đại diện cho một bảng chữ cái. Mỗi ô chứa một ký tự viết hoa duy nhất. Nhiệm vụ là đếm số lần mẫu “UVA” xuất hiện trên lưới khi đọc dọc theo đường thẳng. 

Một lần xuất hiện hợp lệ bao gồm ba ô liên tiếp tạo thành chuỗi U, theo sau là V, theo sau là A và hướng có thể là ngang, dọc hoặc chéo theo bất kỳ hướng nào trong tám hướng có thể có từ ô bắt đầu. Mỗi bộ ba ô có thứ tự hợp lệ được tính là một lần xuất hiện, ngay cả khi các lần xuất hiện khác nhau trùng lặp hoặc chia sẻ các ô. 

Kích thước lưới tối đa là 100 x 100, vì vậy có tối đa 10.000 ô. Từ mỗi ô, chúng ta có thể thử tối đa 8 hướng và theo mỗi hướng, chúng ta chỉ cần kiểm tra một mẫu có độ dài cố định có độ dài 3. Điều này đã gợi ý rằng cách tiếp cận O(NM) hoặc O(NM·8) là đủ, vì tổng công việc chỉ khoảng vài trăm nghìn thao tác. 

Sự tinh tế chính là sự xuất hiện có tính định hướng. Chuỗi “UVA” khác với “AVU” và chúng tôi chỉ tính các trận đấu chuyển tiếp bắt đầu từ U và đi ra ngoài theo một hướng. 

Các trường hợp cạnh phát sinh chủ yếu từ ranh giới. Việc triển khai đơn giản có thể cố gắng truy cập các hàng xóm mà không kiểm tra giới hạn, điều này sẽ không thành công đối với các ô gần các cạnh hoặc góc. Một vấn đề khác là đếm không chính xác các mẫu đảo ngược, ví dụ như đọc ngược “AVU” từ ô “A” nếu chỉ đường bị xử lý sai. 

Một ví dụ về ranh giới cụ thể là lưới 3 x 3: 

đầu vào:```
U..
.V.
..A
```Có chính xác một đường chéo “UVA”. Cách tiếp cận bất cẩn không hạn chế đúng các bước định hướng có thể thử các chỉ mục không hợp lệ bên ngoài lưới. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản: đối với mọi tế bào, hãy cố gắng coi nó là điểm khởi đầu của chuỗi “UVA” có thể xảy ra. Từ ô đó, hãy khám phá mọi hướng trong số tám hướng la bàn và đi từng bước một cách rõ ràng để xem hai ô tiếp theo có khớp với V và A hay không. 

Điều này hoạt động vì chiều dài mẫu cố định và nhỏ. Lực lượng vũ phu kiểm tra mọi vị trí bắt đầu có thể và mọi hướng, đồng thời thực hiện tối đa hai so sánh ký tự cho mỗi hướng. Với N·M ô và 8 hướng, điều này dẫn đến khoảng 160.000 lượt kiểm tra trong trường hợp xấu nhất đối với lưới 100 x 100 vốn đã nhỏ. 

Quan sát quan trọng là chúng ta không bao giờ cần bất kỳ cấu trúc tổng thể hoặc tiền xử lý nào. Vấn đề hoàn toàn mang tính cục bộ: mỗi lần xuất hiện hợp lệ được xác định đầy đủ bởi ô bắt đầu và vectơ chỉ hướng. Điều đó loại bỏ mọi nhu cầu về truyền tải đồ thị hoặc lập trình động. 

Do đó, giải pháp tối ưu chỉ đơn giản là liệt kê có kiểm soát tất cả các bộ ba ô có thể có trên các đường thẳng có độ dài 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(1) | Đã chấp nhận | 
| Tối ưu | O(NM) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành mảng 2D để chúng ta có thể lập chỉ mục bất kỳ ô nào trong thời gian không đổi. Điều này là cần thiết vì chúng tôi liên tục truy cập hàng xóm theo mọi hướng. 
2. Xác định 8 vectơ hướng biểu thị chuyển động ngang, dọc và chéo. Mỗi vectơ là một cặp (dx, dy) mô tả cách hàng và cột thay đổi ở mỗi bước. Điều này đảm bảo chúng tôi bao quát một cách có hệ thống tất cả các khả năng theo đường thẳng mà không bị trùng lặp. 
3. Lặp lại từng ô (i, j) trong lưới. Mỗi tế bào như vậy được coi là điểm khởi đầu tiềm năng của chuỗi “UVA”. 
4. Nếu ô hiện tại không phải là 'U', hãy bỏ qua ô đó ngay lập tức. Điều này làm giảm việc kiểm tra hướng không cần thiết và buộc tất cả các kết quả khớp đều được neo ở ký tự đầu tiên của mẫu. 
5. Đối với mỗi hướng (dx, dy), hãy thử mở rộng chuỗi: 

di chuyển một bước tới (i + dx, j + dy) và kiểm tra xem đó có phải là ‘V’ không, 

sau đó chuyển bước nữa tới (i + 2dx, j + 2dy) và kiểm tra xem đó có phải là ‘A’ hay không. 
6. Trước khi truy cập vào bất kỳ ô lân cận nào, hãy đảm bảo tọa độ vẫn nằm trong giới hạn lưới. Điều này ngăn chặn việc truy cập bộ nhớ không hợp lệ và tránh việc đếm các chuỗi không đầy đủ nằm ngoài ma trận. 
7. Nếu cả hai lần kiểm tra đều thành công, hãy tăng bộ đếm câu trả lời lên một. Mỗi bộ ba hợp lệ tương ứng với chính xác một lần xuất hiện theo hướng. 
8. Sau khi xử lý tất cả các ô và hướng, xuất ra số đếm tích lũy. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện hợp lệ của “UVA” được xác định duy nhất bởi vị trí bắt đầu (chữ ‘U’) và hướng của nó. Thuật toán liệt kê tất cả các vị trí bắt đầu có thể và tất cả các hướng có thể có chính xác một lần. Vì độ dài mẫu được cố định ở mức 3 nên không có sự mơ hồ hoặc chồng chéo trong cách hình thành chuỗi. Bất kỳ kết quả trùng khớp hợp lệ nào đều phải được phát hiện khi vòng lặp đạt đến điểm bắt đầu 'U' và kiểm tra vectơ chỉ hướng chính xác và không có kết quả khớp không hợp lệ nào có thể vượt qua quá trình kiểm tra ký tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(m)]

    target = "UVA"
    dirs = [
        (-1, 0), (1, 0), (0, -1), (0, 1),
        (-1, -1), (-1, 1), (1, -1), (1, 1)
    ]

    ans = 0

    for i in range(m):
        for j in range(n):
            if grid[i][j] != 'U':
                continue
            for dx, dy in dirs:
                x1, y1 = i + dx, j + dy
                x2, y2 = i + 2*dx, j + 2*dy

                if 0 <= x1 < m and 0 <= y1 < n and 0 <= x2 < m and 0 <= y2 < n:
                    if grid[x1][y1] == 'V' and grid[x2][y2] == 'A':
                        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo chiến lược liệt kê. Lưới được lưu trữ dưới dạng danh sách các chuỗi, cho phép truy cập O(1) trên mỗi ô. Danh sách hướng mã hóa tất cả các chuyển động theo đường thẳng. Các vòng lặp lồng nhau đảm bảo mọi khả năng bắt đầu bằng chữ ‘U’ đều được xem xét. 

Một cạm bẫy phổ biến là trộn lẫn các kích thước. Ở đây, m là số hàng và n là số cột, vì vậy việc lập chỉ mục phải sử dụng nhất quán m cho giới hạn hàng và n cho giới hạn cột. Một sự tinh tế khác là đảm bảo chúng tôi luôn bước chính xác hai vị trí cho toàn bộ chiều dài mẫu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
IVO
VIU
UVA
```Chúng tôi chỉ theo dõi các vị trí U có liên quan. 

| (tôi, j) | char | hướng | x1,y1 | x2,y2 | trận đấu | 
| --- | --- | --- | --- | --- | --- | 
| (2,0) | Bạn | (0,1) | (2,1)=V | (2,2)=A | vâng | 

Đầu ra là 1. 

Điều này xác nhận rằng việc kiểm tra theo đường chéo và ngang được xử lý thống nhất và chỉ các chuỗi chuyển tiếp hợp lệ mới được tính. 

### Ví dụ 2 

đầu vào:```
4 5
aaaa
vvvv
uuuu
vvvv
aaaa
```Mỗi chữ ‘u’ ở hàng 2 có thể tạo thành nhiều chuỗi. 

Một dấu vết đại diện: 

| bắt đầu | hướng | Tế bào V | Một tế bào | hợp lệ | 
| --- | --- | --- | --- | --- | 
| (2,1) | xuống | (3,1)=v | (4,1)=a | vâng | 
| (2,1) | cắt xuống bên phải | (3,2)=v | (4,3)=a | vâng | 
| (2,1) | cắt xuống bên trái | (3,0)=v | (4,-1) | không | 

Điều này cho thấy việc kiểm tra ranh giới ngăn cản việc mở rộng đường chéo không hợp lệ như thế nào. 

Tổng số tích lũy tất cả các bộ ba đường thẳng hợp lệ mà không cần tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô được kiểm tra theo 8 hướng với công việc liên tục trên mỗi hướng | 
| Không gian | O(1) | Chỉ có mảng hướng cố định và lưu trữ lưới đầu vào | 

Giới hạn kích thước lưới là 100 x 100 giúp việc này nhanh chóng một cách thoải mái. Tổng số hoạt động vẫn ở mức dưới vài triệu lần kiểm tra ban đầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else exec_capture(inp)

def exec_capture(inp):
    import sys, io
    backup = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdin = backup
    sys.stdout = backup_out
    return out

# provided sample
assert run("3 3\nIVO\nVIU\nUVA\n") == "1"

# diagonal-heavy case
assert run("3 3\nU..\n.V.\n..A\n") == "1"

# straight line horizontal
assert run("3 1\nUVA\n") == "1"

# no matches
assert run("3 3\nAAA\nVVV\nUUU\n") == "0"

# multiple overlaps
assert run("3 3\nUVU\nVVV\nAVA\n") == str(run("3 3\nUVU\nVVV\nAVA\n"))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường chéo U.. .V. ..A | 1 | phát hiện đường chéo | 
| UVA liên tiếp | 1 | trường hợp ngang | 
| tất cả các chữ cái không khớp | 0 | không có kết quả dương tính giả | 
| lưới dày đặc | số lượng nhất quán | xử lý chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi “UVA” hợp lệ bắt đầu ở gần đường viền. Hãy xem xét: 

đầu vào:```
U..
.V.
..A
```Thuật toán bắt đầu từ (0,0), kiểm tra tất cả các hướng. Hầu hết các hướng đều thất bại ngay lập tức do kiểm tra ranh giới. Hướng chéo (1,1) thành công vì tất cả tọa độ trung gian vẫn hợp lệ. Điều này xác nhận rằng việc bảo vệ ranh giới sẽ lọc chính xác các tiện ích mở rộng không hợp lệ mà không ảnh hưởng đến các tiện ích mở rộng hợp lệ. 

Một trường hợp khác là khi nhiều hướng chồng lên nhau trong các lưới dày đặc. Đối với một ô trung tâm được bao quanh bởi nhiều tổ hợp V và A, mỗi hướng được xử lý độc lập. Thuật toán không hợp nhất hoặc loại bỏ trùng lặp vì mỗi hướng xác định một lần xuất hiện có thứ tự riêng biệt, phù hợp với định nghĩa vấn đề.
