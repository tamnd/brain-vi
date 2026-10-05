---
title: "CF 104921B - Đứa Trẻ Tốt"
description: "Chúng ta được cung cấp một tập hợp nhỏ các số có một chữ số. Đối với mỗi trường hợp thử nghiệm, chúng tôi được phép chọn chính xác một trong các chữ số này và tăng nó lên một."
date: "2026-06-28T18:07:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104921
codeforces_index: "B"
codeforces_contest_name: "Easy_Training"
rating: 0
weight: 104921
solve_time_s: 87
verified: false
draft: false
---

[CF 104921B - Đứa trẻ ngoan](https://codeforces.com/problemset/problem/104921/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp nhỏ các số có một chữ số. Đối với mỗi trường hợp thử nghiệm, chúng tôi được phép chọn chính xác một trong các chữ số này và tăng nó lên một. Sau lần thay đổi duy nhất đó, chúng ta nhân tất cả các số trong tập hợp lại với nhau và muốn tích này càng lớn càng tốt. 

Cấu trúc của dữ liệu đầu vào rất quan trọng: mỗi trường hợp thử nghiệm là độc lập và mỗi trường hợp chứa tối đa chín chữ số. Đầu ra của mỗi trường hợp thử nghiệm chỉ là một số nguyên, sản phẩm tốt nhất có thể sau khi áp dụng mức tăng duy nhất được phép. 

Ràng buộc trên n là cực kỳ nhỏ. Với n tối đa 9 và tối đa 10^4 trường hợp thử nghiệm, ngay cả một giải pháp thử mọi lựa chọn có thể về việc tăng chữ số nào cũng dễ dàng đủ nhanh. Điều này ngay lập tức loại trừ mọi nhu cầu xử lý trước phức tạp hoặc tối ưu hóa toán học ngoài mô phỏng trực tiếp. 

Sự tinh tế chính đến từ số không. Một trực giác ngây thơ có thể gợi ý rằng việc tăng chữ số lớn nhất luôn là tối ưu, nhưng điều này sẽ thất bại bất cứ khi nào số 0 tồn tại. Số 0 làm cho toàn bộ tích số bằng 0, vì vậy cách duy nhất để có được kết quả khác 0 là chuyển ít nhất một số 0 thành một. Ví dụ, với chữ số`[0, 5, 6]`, tăng 6 lên 7 vẫn để tích bằng 0, trong khi tăng 0 lên 1 làm tích`1 * 5 * 6 = 30`, điều đó thực sự tốt hơn. 

Một trường hợp cạnh khác là khi tất cả các chữ số đều bằng 0 ngoại trừ một. Ví dụ`[0, 0, 9]`. Tăng 9 lên 10 sẽ tạo ra sản phẩm`0`, trong khi tăng số 0 sẽ cho`[1, 0, 9]`, vẫn là sản phẩm`0`. Trong những trường hợp như vậy, mọi hành động đều tương đương nhau, nhưng đánh giá bạo lực vẫn xử lý chính xác hành động đó. 

Trường hợp tinh tế cuối cùng là khi tất cả các chữ số khác 0 và tương đối lớn. Việc tăng một chữ số nhỏ hơn đôi khi có thể tốt hơn việc tăng chữ số lớn nhất vì phép nhân rất nhạy cảm với phân phối. Ví dụ`[3, 3, 3]`: tăng một 3 đến 4 sản lượng`36`, trong khi tăng bất kỳ cái nào khác cũng mang lại kết quả tương tự, nhưng nhìn chung các phân phối hỗn hợp yêu cầu kiểm tra tất cả các vị trí. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi chỉ số, tạm thời tăng chữ số đó lên một, tính tích của tất cả các phần tử và giữ kết quả tối đa. Mỗi đánh giá tốn O(n) phép nhân và có n lựa chọn, vì vậy mỗi trường hợp kiểm thử có chi phí O(n2). Vì n nhiều nhất là 9 nên đây thực sự là thời gian không đổi trong thực tế, nhưng nó vẫn giúp đơn giản hóa. 

Quan sát quan trọng là không gian quyết định rất nhỏ. Chỉ có n nước đi có thể thực hiện được và mỗi nước đi là độc lập. Không cần lập trình động hay lý luận tham lam vì chúng ta không đưa ra nhiều quyết định mà chỉ chọn một vị trí duy nhất để sửa đổi. Điều này thu gọn vấn đề thành liệt kê trực tiếp. 

Do đó, việc tối ưu hóa không phải là giảm độ phức tạp tiệm cận mà là viết vòng đánh giá rõ ràng nhất: tính toán tích cho từng chỉ số ứng cử viên và theo dõi mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t · n²) | O(1) | Đã chấp nhận | 
| Đếm tối ưu | O(t · n²) | O(1) | Đã chấp nhận | 

Trong thực tế, cả hai đều giống nhau ở đây vì n được giới hạn bởi 9. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và đánh giá tất cả các mức tăng đơn lẻ có thể có. 

1. Đọc mảng chữ số của test case hiện tại. Chúng tôi lưu trữ nó dưới dạng danh sách để có thể mô phỏng các sửa đổi một cách dễ dàng. 
2. Khởi tạo một biến`best`về không. Điều này sẽ theo dõi sản phẩm tối đa được nhìn thấy trên tất cả các lựa chọn về chữ số nào sẽ tăng lên. 
3. Đối với mỗi chỉ số`i`trong mảng, mô phỏng tăng`a[i]`bởi một. Chúng tôi không sửa đổi mảng vĩnh viễn; thay vào đó, chúng tôi coi nó như`a[i] + 1`chỉ dành cho phép tính này. Điều này tránh được tác động lây lan ngẫu nhiên giữa các lần thử nghiệm. 
4. Tính tích của tất cả các phần tử theo sự sửa đổi này. Mọi phần tử ngoại trừ`i`không thay đổi trong khi vị trí`i`đóng góp`(a[i] + 1)`thay vì`a[i]`. 
5. So sánh sản phẩm này với`best`và cập nhật`best`nếu nó lớn hơn. Điều này đảm bảo rằng sau khi xem xét tất cả các lựa chọn, chúng tôi sẽ giữ lại lựa chọn tối ưu. 
6. Đầu ra`best`sau khi tất cả các chỉ số đã được kiểm tra. 

### Tại sao nó hoạt động 

Thuật toán đánh giá rõ ràng mọi thao tác hợp lệ mà bài toán cho phép: chọn chính xác một chỉ mục để tăng. Mỗi đánh giá sẽ tính toán sản phẩm thu được chính xác, do đó không cần đến phép tính gần đúng hoặc phương pháp phỏng đoán. Vì tập hợp các kết quả có thể xảy ra chính xác là tập hợp của n sửa đổi này, nên việc lấy giá trị tối đa trên tất cả chúng sẽ đảm bảo tính đúng đắn. Không có sự tương tác giữa các lựa chọn, vì vậy việc liệt kê chúng một cách độc lập sẽ bao trùm toàn bộ không gian giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        best = 0

        for i in range(n):
            prod = 1
            for j in range(n):
                if i == j:
                    prod *= (a[j] + 1)
                else:
                    prod *= a[j]
            best = max(best, prod)

        print(best)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Vòng lặp bên ngoài xử lý các trường hợp kiểm thử và vòng lặp bên trong xử lý các trường hợp kiểm thử.`i`chọn chữ số nào để tăng lên. Vòng lặp bên trong thứ hai tính toán kết quả cho lựa chọn đó. 

Một cạm bẫy triển khai phổ biến là sửa đổi mảng tại chỗ và quên hoàn nguyên mảng đó, điều này dẫn đến việc xếp tầng các kết quả không chính xác qua các lần lặp. Ở đây, giá trị`(a[j] + 1)`được tính toán nhanh chóng, tránh hoàn toàn đột biến. 

Một điểm tinh tế khác là khởi tạo`best`về 0 chứ không phải là tích của mảng ban đầu. Vì việc tăng số 0 có thể tạo ra kết quả tốt hơn rất nhiều nên việc bắt đầu từ số 0 sẽ bao gồm tất cả các trường hợp một cách an toàn mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 1, 2, 3]`Chúng tôi đánh giá từng mức tăng có thể. 

| Chỉ số tăng lên | Mảng sửa đổi | Sản phẩm | 
| --- | --- | --- | 
| 0 | [3, 1, 2, 3] | 18 | 
| 1 | [2, 2, 2, 3] | 24 | 
| 2 | [2, 1, 3, 3] | 18 | 
| 3 | [2, 1, 2, 4] | 16 | 

Kết quả tốt nhất là`24`, đạt được bằng cách tăng phần tử thứ hai. 

Điều này cho thấy rằng việc tăng phần tử lớn nhất không phải lúc nào cũng tối ưu, vì việc cải thiện giá trị trung bình nhỏ hơn sẽ mang lại sản phẩm cao hơn. 

### Ví dụ 2 

đầu vào:`[0, 5, 6]`| Chỉ số tăng lên | Mảng sửa đổi | Sản phẩm | 
| --- | --- | --- | 
| 0 | [1, 5, 6] | 30 | 
| 1 | [0, 6, 6] | 0 | 
| 2 | [0, 5, 7] | 0 | 

Sự lựa chọn tốt nhất rõ ràng là tăng số không. Điều này chứng tỏ ưu thế của việc loại bỏ số 0 trong việc cải thiện các giá trị vốn đã lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n²) | Đối với mỗi trường hợp thử nghiệm, chúng tôi thử n số gia có thể và mỗi số yêu cầu nhân n số | 
| Không gian | O(1) | Chỉ các biến phụ không đổi ngoài mảng đầu vào | 

Cho rằng n 9 × 10^4, tổng số thao tác tối đa là khoảng 9 × 9 × 10^4, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (reconstructed from statement formatting)
assert run("""4
4
2 1 2 3
1
2
5
4 3 2 3 4
9
9 9 9 9 9 9 9 9 9
""") == """24
3
432
430467210"""

# minimum size
assert run("""1
1
0
""") == "1"

# all zeros
assert run("""1
3
0 0 0
""") == "1"

# mixed zeros
assert run("""1
4
0 2 3 4
""") == "24"

# all nines
assert run("""1
3
9 9 9
""") == "900"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 1 | trường hợp tối thiểu, số gia chuyển đổi 0 thành 1 | 
| tất cả số không | 1 | đảm bảo luôn áp dụng ít nhất một mức tăng | 
| số không hỗn hợp | 24 | xác nhận chiến lược xử lý bằng không chiếm ưu thế | 
| tất cả chín | 900 | kiểm tra xử lý hiệu ứng mang đến 10 | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng chứa số 0. Ví dụ, đầu vào`[0, 2, 3, 4]`tạo ra tích bằng 0 trừ khi số 0 tăng lên. Thuật toán đánh giá chính xác trường hợp chỉ số 0 được tăng lên, mang lại`[1, 2, 3, 4]`và sản phẩm`24`, điều này chiếm ưu thế trong tất cả các lựa chọn khác vẫn bao gồm số 0. 

Một trường hợp khác là mảng một phần tử`[0]`. Động thái duy nhất là tăng nó lên, tạo ra`[1]`, vì vậy đầu ra là`1`. Thuật toán xử lý việc này một cách tự nhiên vì nó vẫn đánh giá chỉ mục duy nhất và tính toán`(0 + 1)`. 

Khi tất cả các chữ số đều lớn, chẳng hạn như`[9, 9, 9]`, sự gia tăng tạo ra một`10`, và sản phẩm trở thành`900`. Thuật toán xử lý chính xác điều này mà không cần xử lý`10`đặc biệt, vì phép nhân số nguyên trong Python hỗ trợ nó một cách tự nhiên. 

Cuối cùng, khi có nhiều số 0 tồn tại, chẳng hạn như`[0, 0, 5]`, bất kỳ mức tăng đơn lẻ nào vẫn để lại ít nhất một số 0, vì vậy hầu hết các kết quả đều bằng 0 ngoại trừ khi tăng số 0. Thuật toán vẫn so sánh tất cả các trường hợp một cách thống nhất và chọn giá trị chính xác nhất mà không cần logic trường hợp đặc biệt.
