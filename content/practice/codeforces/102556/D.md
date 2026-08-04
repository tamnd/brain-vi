---
title: "CF 102556D - Riana và phân phối bánh"
description: "Gọi phần trăm được chọn của người thứ i là Pi, được viết dưới dạng phân số từ 0 đến 1. Khi người thứ i đến lượt, có hai điều xảy ra. Họ lấy Pi của chiếc bánh còn nguyên, sau đó họ cũng lấy Pi của từng miếng bánh đã thuộc sở hữu của những người trước đó."
date: "2026-08-03T19:25:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102556
codeforces_index: "D"
codeforces_contest_name: "2020 Ateneo de Manila University DISCS PrO HS Division"
rating: 0
weight: 102556
solve_time_s: 150
verified: true
draft: false
---

[CF 102556D - Riana và phân phối bánh](https://codeforces.com/problemset/problem/102556/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy để tỷ lệ phần trăm được chọn của`i`-là người thứ`P_i`, được viết dưới dạng phân số giữa`0`Và`1`. 

Khi người`i`đến lượt, có hai điều xảy ra. Họ lấy`P_i`của chiếc bánh còn nguyên, sau đó họ cũng lấy`P_i`của mỗi phần đã được sở hữu bởi những người trước đó. Sau khi mọi người chơi xong, phần còn lại của chiếc bánh phải bằng 0 và chúng tôi muốn kích thước lát cuối cùng bằng nhau nhất có thể. 

Đầu vào chỉ chứa số lượng người. Kết quả đầu ra là tỷ lệ được mỗi người chọn theo thứ tự. 

Hạn chế duy nhất là`N ≤ 1000`, vì vậy ngay cả một`O(N^2)`mô phỏng sẽ dễ dàng phù hợp. Thách thức không phải là tính hiệu quả mà là việc khám phá cấu trúc toán học của quy trình. 

Trường hợp cạnh không rõ ràng đầu tiên là`N = 1`. Câu trả lời hợp lệ duy nhất là`100%`, bởi vì nếu không thì một số chiếc bánh vẫn chưa được ăn. 

Trường hợp tế nhị thứ hai là bắt người cuối cùng phải chọn`100%`không cần thiết. Ví dụ, với`N = 2`, đang chọn`100%, 50%`không để lại chiếc bánh nào vì người chơi đầu tiên đã ăn hết chiếc bánh đó. Một chiến lược tham lam luôn buộc người chơi cuối cùng phải lấy mọi thứ sẽ bỏ lỡ sự phân phối tối ưu. 

Cái bẫy cuối cùng là nhầm lẫn giữa tỷ lệ phần trăm người chơi chọn với tỷ lệ phần trăm cuối cùng họ sở hữu. Những người chơi sau liên tục ăn trộm của những người trước đó nên tỷ lệ phần trăm được chọn và phần chia sẻ cuối cùng có số lượng khác nhau. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp giữ lại lát cắt hiện tại của mọi người chơi. Bất cứ khi nào có người chơi mới đến, mỗi lát cắt trước đó sẽ được nhân với`(1 - P_i)`, người chơi mới sẽ nhận được mọi thứ đã bị xóa khỏi các lát cắt đó cộng với`P_i`của chiếc bánh chưa được chạm tới và chiếc bánh chưa được chạm tới cũng được nhân với`(1 - P_i)`. Điều này mô phỏng trò chơi một cách trung thực nhưng không cho chúng ta biết cách chọn tỷ lệ phần trăm. 

Quan sát quan trọng là lợi ích mà người chơi kiếm được từ chiếc bánh chưa được chạm tới và từ việc ăn trộm luôn có giá trị như nhau. 

Giả sử phần chưa được chạm tới trước người`i`là`R`. Tổng số tiền mà người chơi trước đã sở hữu là`1 - R`. Người chơi mới nhận được`P_i · R + P_i · (1 - R) = P_i`. 

Vì vậy ngay sau lượt của mình, người chơi`i`luôn sở hữu chính xác`P_i`của chiếc bánh ban đầu. 

Sau đó, mỗi người chơi sau chỉ cần chia tỷ lệ lát đó theo`(1 - P_j)`. Do đó phần chia cuối cùng là$$F_i=P_i\prod_{j=i+1}^{N}(1-P_j).$$Bây giờ xác định$$S_i=\prod_{j=i}^{N}(1-P_j),$$phần chưa được chạm tới ngay trước người`i`. 

Sau đó$$F_i=S_{i+1}-S_i.$$Chiếc bánh còn sót lại là`S_1`, và yêu cầu không có thức ăn thừa có nghĩa là`S_1=0`. Cũng`S_{N+1}=1`. 

Vì số cổ phiếu cuối cùng cộng lại là`1`, sự khác biệt nhỏ nhất có thể đạt được giữa phần lớn nhất và phần nhỏ nhất đạt được khi mọi phần chia sẻ cuối cùng đều chính xác`1/N`. 

Thiết lập mọi`F_i=1/N`cho$$S_i=\frac{i-1}{N}.$$Phục hồi tỷ lệ phần trăm,$$P_i=\frac{S_{i+1}-S_i}{S_{i+1}}.$$Vì`i=1`,$$P_1=1.$$Đối với mọi`i≥2`,$$P_i=\frac{1/N}{i/N}=\frac1i.$$Vì vậy, câu trả lời rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng và tìm kiếm Brute Force | Hàm mũ | O(N) | Quá chậm | 
| Công thức dạng đóng | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`N`. 
2. In`100.0`cho người đầu tiên. Điều này đảm bảo chiếc bánh chưa được chạm tới ngay lập tức trở thành số 0, thỏa mãn điều kiện không có phần thừa. 
3. Đối với mỗi người`i`từ`2`ĐẾN`N`, in`100 / i`. Đây chính xác là giá trị thu được từ công thức dẫn xuất`P_i = 1/i`. 

Đạo hàm đảm bảo rằng phần chia sẻ cuối cùng của mỗi người chơi trở nên chính xác`1/N`, đó là điều tốt nhất có thể vì tất cả các cổ phần đều có tổng bằng một. 

### Tại sao nó hoạt động 

Người bất biến chính là người chơi đó`i`sở hữu chính xác`P_i`ngay sau khi kết thúc lượt của mình. Mỗi lượt sau chỉ cần nhân mỗi lát cắt hiện có với cùng một hệ số. Điều này đưa ra công thức$$F_i=P_i\prod_{j>i}(1-P_j).$$Viết hậu tố sản phẩm là`S_i`chuyển đổi biểu thức thành$$F_i=S_{i+1}-S_i.$$Các cổ phần cuối cùng bằng nhau xác định duy nhất mọi`S_i`và các sản phẩm hậu tố đó xác định duy nhất mọi phần trăm. Vì các cổ phần bằng nhau làm cho mức tối đa và tối thiểu giống hệt nhau nên không có sự phân phối nào tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    print(f"{100.0:.10f}")
    for i in range(2, n + 1):
        print(f"{100.0 / i:.10f}")

if __name__ == "__main__":
    solve()
```Chương trình in trực tiếp câu trả lời dạng đóng. Giá trị đầu tiên luôn là`100%`. Mọi giá trị sau này chỉ đơn giản là`100/i`. Không cần mô phỏng vì đạo hàm toán học đã mô tả giải pháp tối ưu duy nhất. 

Độ chính xác của dấu phẩy động là quá đủ ở đây. Lỗi bắt buộc là`1e-4`, trong khi in mười chữ số thập phân sẽ giữ cho sai số tích lũy nhỏ hơn nhiều bậc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Người | Phần trăm in | 
| --- | --- | 
| 1 | 100.0000000000 | 

Người chơi duy nhất lấy toàn bộ chiếc bánh, vì vậy việc phân phối cuối cùng đã hoàn toàn bằng nhau. 

### Ví dụ 2 

đầu vào:```
4
```| Người | Tỷ lệ in | 
| --- | --- | 
| 1 | 100.0000000000 | 
| 2 | 50.0000000000 | 
| 3 | 33.3333333333 | 
| 4 | 25.0000000000 | 

Cổ phiếu cuối cùng trở thành 

| Người | Chia sẻ cuối cùng | 
| --- | --- | 
| 1 | 25% | 
| 2 | 25% | 
| 3 | 25% | 
| 4 | 25% | 

Điều này xác nhận rằng mọi người chơi đều kết thúc với đúng một phần tư chiếc bánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một giá trị được in cho mỗi người. | 
| Không gian | O(1) | Chỉ có một biến vòng lặp được lưu trữ. | 

Với tối đa 1000 người, việc này diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve():
    input = sys.stdin.readline
    n = int(input())
    print(f"{100.0:.10f}")
    for i in range(2, n + 1):
        print(f"{100.0 / i:.10f}")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue()

assert run("1\n") == "100.0000000000\n"

assert run("2\n") == (
    "100.0000000000\n"
    "50.0000000000\n"
)

assert run("3\n") == (
    "100.0000000000\n"
    "50.0000000000\n"
    "33.3333333333\n"
)

out = run("1000\n").splitlines()
assert len(out) == 1000
assert abs(float(out[-1]) - 0.1) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`100`| Kích thước đầu vào tối thiểu | 
|`2`|`100`,`50`| Trường hợp không tầm thường đầu tiên | 
|`3`|`100`,`50`,`33.333...`| Công thức chung | 
|`1000`| Giá trị cuối cùng là`0.1`| Kích thước đầu vào lớn nhất | 

## Vỏ cạnh 

Khi nào`N = 1`, thuật toán chỉ in`100`. Chiếc bánh chưa được chạm tới ngay lập tức trở thành số 0 và người chơi duy nhất sở hữu toàn bộ chiếc bánh. Cả hai yêu cầu đều được đáp ứng. 

Khi`N = 2`, thuật toán in`100`Và`50`. Người chơi đầu tiên sở hữu toàn bộ chiếc bánh. Người chơi thứ hai ăn trộm một nửa số đó, để lại cho cả hai người chơi chính xác`50%`. Điều này chứng tỏ người chơi đầu tiên chứ không phải người cuối cùng mới là người phải lựa chọn`100%`. 

Đối với các giá trị lớn hơn như`N = 3`, thuật toán in`100`,`50`, Và`33.333...`. Kết quả chia sẻ cuối cùng đều chính xác`1/3`, xác nhận rằng việc xây dựng sẽ cân bằng quyền sở hữu cuối cùng của mọi người chơi trong khi vẫn không để lại chiếc bánh nào bị ảnh hưởng.
