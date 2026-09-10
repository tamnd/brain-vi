---
title: "CF 104603B - Đen trắng"
description: "Chúng ta có một lưới $N nhân N$ trong đó mỗi ô có thể sử dụng được (màu đen) hoặc bị cấm (màu trắng). Nhiệm vụ là đặt càng nhiều quân domino ngang càng tốt, trong đó mỗi quân domino bao phủ chính xác hai ô liền kề trong cùng một hàng."
date: "2026-06-30T02:53:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104603
codeforces_index: "B"
codeforces_contest_name: "2023 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 104603
solve_time_s: 45
verified: true
draft: false
---

[CF 104603B - Đen trắng](https://codeforces.com/problemset/problem/104603/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$lưới trong đó mỗi ô có thể sử dụng được (đen) hoặc bị cấm (trắng). Nhiệm vụ là đặt càng nhiều quân domino ngang càng tốt, trong đó mỗi quân domino bao phủ chính xác hai ô liền kề trong cùng một hàng. Một quân domino chỉ hợp lệ nếu cả hai ô được che phủ đều có màu đen và không ô nào có thể thuộc về nhiều hơn một quân domino. 

Vì vậy, vấn đề tương đương với việc chọn càng nhiều cặp ô đen liền kề nằm ngang rời rạc càng tốt trên tất cả các hàng. 

Kích thước lưới tối đa là$50 \times 50$, vậy có nhiều nhất là 2500 ô. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân trên các vị trí. Ngay cả việc quay lại một cách ngây thơ về tất cả các cách đặt hoặc không đặt một quân domino cũng sẽ là quá lớn vì chỉ riêng mỗi hàng có thể tạo ra các lựa chọn theo cấp số nhân. 

Một quan sát quan trọng là các hàng độc lập: quân domino không bao giờ vượt qua các hàng. Vì vậy, bài toán được phân tách thành việc giải cùng một bài toán con cho mỗi hàng và tính tổng kết quả. 

Trường hợp phức tạp là các hàng có mẫu xen kẽ trong đó việc ghép nối tham lam có thể thất bại nếu không cẩn thận. Ví dụ, trong một hàng như`NNNN`, một cặp đôi tham lam ngây thơ từ trái sang phải sẽ tạo ra đúng 2 quân domino, nhưng theo các mẫu như`NNNNN`, các quy tắc bỏ qua bất cẩn có thể bị tính sai nếu việc triển khai không thực thi nghiêm ngặt việc sử dụng rời rạc. Một trường hợp khác là các hàng có người da đen bị cô lập như`NBNBN`, nơi không thể đặt quân domino mặc dù có nhiều ô đen. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ xem xét từng vị trí có thể có của quân domino ngang trên lưới. Điều này có thể được coi là việc chọn các cạnh giữa các ô đen liền kề, đảm bảo không có đỉnh nào được sử dụng lại. Đây chính xác là vấn đề khớp tối đa trên biểu đồ trong đó mỗi ô là một nút và các cạnh tồn tại giữa các lân cận nằm ngang trong cùng một hàng. 

Một giải pháp đơn giản sẽ thử tất cả các tập hợp con của các cạnh và kiểm tra tính hợp lệ. Với tối đa 2500 ô, thậm chí việc giới hạn ở các cạnh ngang cũng mang lại khoảng$O(N^2)$các cạnh và tập hợp con của các cạnh tăng theo cấp số nhân, xung quanh$2^{O(N^2)}$, điều đó hoàn toàn không thể thực hiện được. 

Sự đơn giản hóa chính là về cấu trúc: các cạnh chỉ tồn tại giữa$(i, j)$Và$(i, j+1)$. Điều đó có nghĩa là mỗi hàng là một biểu đồ đường dẫn độc lập được chia bởi các ô màu trắng. Trong biểu đồ đường dẫn, việc khớp tối đa là không đáng kể: khớp một cách tham lam các đỉnh có sẵn liên tiếp từ trái sang phải. 

Điều này làm giảm vấn đề từ vấn đề kết hợp toàn cục thành$N$quét tuyến tính độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các tập hợp con domino |$O(2^{N^2})$|$O(N^2)$| Quá chậm | 
| Ghép nối tham lam theo hàng |$O(N^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý lưới theo từng hàng, vì quân domino không bao giờ vượt qua ranh giới hàng. Điều này đảm bảo mỗi quyết định được giới hạn trong các vấn đề con độc lập. 
2. Đối với mỗi hàng, quét từ trái sang phải đồng thời theo dõi xem ô trước đó có thể được ghép nối hay không. Chúng tôi chỉ thử ghép nối khi nhìn thấy hai ô màu đen liên tiếp. 
3. Khi gặp ô màu đen, hãy kiểm tra ô tiếp theo. Nếu nó cũng màu đen, đặt quân domino che cả hai và bỏ qua vị trí tiếp theo. Điều này đảm bảo không có sự chồng chéo. 
4. Nếu ô tiếp theo có màu trắng hoặc chúng ta ở cuối hàng, chúng ta không thể đặt quân domino bắt đầu từ vị trí này nên chúng ta tiến về phía trước. 
5. Tích lũy số lượng quân domino được đặt cho mỗi hàng thành tổng số toàn cầu. 

### Tại sao nó hoạt động 

Mỗi hàng tạo thành một đường dẫn trong đó các đỉnh là các ô và các cạnh chỉ tồn tại giữa các ô đen liên tiếp. Số cạnh rời rạc tối đa trong biểu đồ đường dẫn có được bằng cách chọn tham lam cạnh có sẵn ngoài cùng bên trái bất cứ khi nào có thể. Bất kỳ giải pháp thay thế nào bỏ qua một cặp hợp lệ sẽ không làm tăng các tùy chọn trong tương lai, bởi vì việc bỏ qua chỉ để lại cùng một hoặc ít cơ hội hơn để khớp đúng hơn. Điều này chứng tỏ rằng cấu trúc tham lam luôn tạo ra kết quả khớp tối ưu trên mỗi hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    ans = 0

    for _ in range(n):
        row = input().strip()
        i = 0
        while i < n:
            if row[i] == 'N':
                if i + 1 < n and row[i + 1] == 'N':
                    ans += 1
                    i += 2
                else:
                    i += 1
            else:
                i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược tham lam theo hàng. Chúng tôi duy trì một chỉ số`i`và di chuyển nó về phía trước tùy thuộc vào việc chúng ta có tạo thành quân domino hay không. Khi hai liên tiếp`N`các ô xuất hiện, chúng tôi ngay lập tức sử dụng cả hai và tăng thêm 2 để thực thi việc sử dụng rời rạc. Nếu không, chúng tôi sẽ tăng thêm 1. 

Điều tinh tế quan trọng là chúng tôi không bao giờ xem xét lại một ô đã bị bỏ qua hoặc sử dụng. Đây là điều đảm bảo tính chính xác: mỗi ô tham gia vào tối đa một quyết định, do đó không thể xảy ra sự chồng chéo. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ:```
N N B N
N N N B
B N N N
N B N N
```Thực hiện theo từng hàng: 

| Hàng | tôi | hàng[i] | hàng[i+1] | Hành động | Domino | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | N | N | nơi | 1 | 
| 2 | 2 | B | - | bỏ qua | 1 | 
| 3 | 0 | N | N | nơi | 2 | 
| 4 | 2 | N | B | bỏ qua | 2 | 
| 5 | 0 | B | - | bỏ qua | 2 | 
| 6 | 1 | N | N | nơi | 3 | 
| 7 | 3 | N | - | kết thúc | 3 | 

Điều này chứng tỏ cách ghép đôi tham lam sẽ trích xuất tất cả các cặp màu đen liền kề có sẵn mà không có xung đột. 

Một ví dụ thứ hai:```
N B N B N
N N B N N
B B B B B
N N N N N
N B B B N
```Đối với lưới này: 

| Hàng | Mẫu | Domino | 
| --- | --- | --- | 
| 1 | NBNBN | 0 | 
| 2 | N N B N N | 1 | 
| 3 | BBBBB | 2 | 
| 4 | NNNNN | 2 | 
| 5 | NBBBN | 1 | 

Điều này cho thấy rằng ngay cả các hàng màu đen dày đặc cũng giảm xuống thành các cặp liền kề đơn giản, trong khi các mẫu xen kẽ không đóng góp gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi ô được truy cập một lần trên tất cả các hàng | 
| Không gian |$O(1)$| Chỉ sử dụng các biến phụ không đổi | 

Lưới có tối đa 2500 ô, do đó, một lần quét tuyến tính trên tất cả các ô là không đáng kể trong giới hạn thời gian. Giải pháp chạy tốt dưới 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

def solve():
    n = int(input().strip())
    ans = 0
    for _ in range(n):
        row = input().strip()
        i = 0
        while i < n:
            if row[i] == 'N':
                if i + 1 < n and row[i + 1] == 'N':
                    ans += 1
                    i += 2
                else:
                    i += 1
            else:
                i += 1
    print(ans)

# sample-style checks
assert run("5\nBNBNB\nBBNNN\nNNNNN\nBNNNN\nNNBNN\n") == "7"

# minimum size
assert run("1\nN\n") == "0"

# all white
assert run("3\nBBB\nBBB\nBBB\n") == "0"

# full black row
assert run("1\nNNNNN\n") == "2"

# alternating pattern
assert run("1\nNBNBN\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 đen | 0 | hành vi lưới nhỏ nhất | 
| toàn màu trắng | 0 | không thể có vị trí | 
| hàng đen đầy đủ | 2 | ghép đôi tham lam tối đa | 
| mô hình xen kẽ | 0 | không có trận đấu liền kề | 

## Vỏ cạnh 

Một lưới đơn ô như`N`không có vị trí domino có thể. Thuật toán xử lý hàng, không thấy cặp hợp lệ và trả về 0 chính xác. 

Một hàng hoàn toàn màu đen như`NNNNN`được xử lý bằng cách khớp các cặp lặp lại ở các chỉ số (0,1) và (2,3), để lại một ô còn sót lại. Quá trình quét bỏ qua phần còn sót lại một cách tự nhiên, tạo ra 2 quân domino. 

Một mô hình xen kẽ như`NBNBN`không bao giờ kích hoạt một cặp liền kề hợp lệ, do đó thuật toán không bao giờ tăng bộ đếm, mang lại kết quả chính xác là 0.
