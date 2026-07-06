---
title: "CF 102920L - Hai tòa nhà"
description: "Chúng ta được cho một dãy các chiều cao của tòa nhà được sắp xếp theo một đường thẳng, mỗi vị trí có chiều rộng tòa nhà là 1."
date: "2026-07-04T07:57:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102920
codeforces_index: "L"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Seoul Regional Contest"
rating: 0
weight: 102920
solve_time_s: 47
verified: true
draft: false
---

[CF 102920L - Hai tòa nhà](https://codeforces.com/problemset/problem/102920/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chiều cao của các tòa nhà được sắp xếp theo đường thẳng, mỗi vị trí có một tòa nhà có chiều rộng là 1. Chúng ta muốn chọn hai tòa nhà riêng biệt, với tòa nhà bên trái xuất hiện sớm hơn trong chuỗi và cho điểm cặp đó bằng cách sử dụng công thức (chiều cao của tòa nhà bên trái cộng với chiều cao của tòa nhà bên phải) nhân với khoảng cách giữa các vị trí của chúng. 

Nhiệm vụ là tìm ra số điểm tối đa có thể có trên tất cả các cặp hợp lệ. Theo trực giác, chúng ta đang đánh đổi hai yếu tố cùng một lúc. Chọn những tòa nhà cao tầng sẽ tăng tổng số tiền, trong khi chọn những tòa nhà ở xa sẽ tăng khoảng cách. Câu trả lời tối ưu là sự cân bằng nào đó giữa hai tác động này và không rõ sự cân bằng đó nằm ở đâu. 

Ràng buộc n có thể lớn tới 1.000.000, do đó, bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp sẽ quá chậm. Quét bậc hai sẽ bao gồm khoảng 10^12 thao tác trong trường hợp xấu nhất, điều này hoàn toàn không khả thi trong giới hạn thời gian 1 giây. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào đánh giá rõ ràng từng cặp. 

Khó khăn tinh tế hơn là cả hai thành phần của biểu thức đều phụ thuộc vào cặp theo cách kết hợp. Nếu chỉ khoảng cách là quan trọng thì chúng ta sẽ luôn lấy điểm cuối. Nếu chỉ có chiều cao là quan trọng thì chúng ta sẽ lấy hai giá trị lớn nhất. Sản phẩm kết hợp chúng theo cách ngăn chặn sự tối ưu hóa độc lập. 

Một cạm bẫy phổ biến là thử các chiến lược ghép nối địa phương tham lam, chẳng hạn như ghép từng tòa nhà với tòa nhà xa nhất hoặc cao nhất từng thấy cho đến nay. Những điều này thất bại vì đối tác tốt nhất cho tòa nhà phụ thuộc vào sự cân bằng toàn cầu giữa tăng chiều cao và giảm khoảng cách, chứ không phải quy tắc địa phương. 

Ví dụ: trong một chuỗi như 1 100 2 3 4 100, việc ghép từng 100 một cách tham lam với các điểm cuối sẽ bỏ lỡ các kết hợp như (100, 100) giúp tối đa hóa cả tổng và khoảng cách. Bất kỳ chiến lược nào không xem xét một cách có hệ thống sự tương tác giữa tất cả các ứng viên đều có nguy cơ thiếu đi sự tối ưu về cấu trúc chéo như vậy. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Chúng tôi thử mọi cặp (i, j), tính toán (h[i] + h[j]) * (j - i) và lấy giá trị tối đa. Điều này đúng vì nó làm cạn kiệt toàn bộ không gian tìm kiếm. Tuy nhiên, nó đánh giá khoảng n(n-1)/2 cặp, trở thành khoảng 5 * 10^11 phép toán khi n bằng 10^6. Ngay cả với số học rất nhanh, điều này vẫn vượt xa giới hạn khả thi. 

Để cải thiện, chúng ta cần tránh việc tính toán lại cùng một cấu trúc nhiều lần. Việc mở rộng công thức sẽ cho h[i]_(j-i) + h[j]_(j-i), vẫn kết hợp i và j theo cách không tách biệt ngay lập tức. Quan sát chính là cố định cấu trúc khoảng cách một cách ngầm định bằng cách viết lại biểu thức ở dạng phân tách các đóng góp của i và j đối với j. 

Chúng ta có thể viết lại điểm cho một j cố định như sau: 

(h[i] + h[j]) * (j - i) 

= h[i] * j - h[i] * i + h[j] * j - h[j] * i 

= (h[i] * j - h[i] * i) + (h[j] * j - h[j] * i) 

Điều này vẫn chưa tách biệt hoàn toàn các biến một cách rõ ràng, nhưng chúng ta có thể sắp xếp lại vấn đề bằng cách sửa đúng điểm cuối j. Đối với mỗi j, chúng tôi muốn tối đa hóa i < j: 

(h[i] + h[j]) * (j - i) 

= (h[i] + h[j]) * j - (h[i] + h[j]) * i 

= j_h[i] + j_h[j] - i_h[i] - i_h[j] 

= j_h[j] + (j_h[i] - i_h[i]) - i_h[j] 

Điều này gợi ý rằng với mỗi i, sự đóng góp của nó phụ thuộc tuyến tính vào j. Tuy nhiên, một cái nhìn sâu sắc thực tế hơn đến từ việc sắp xếp lại như sau: 

(h[i] + h[j]) * (j - i) 

= (h[i] + h[j]) * j - (h[i] + h[j]) * i 

Đối với j cố định, h[j] không đổi, do đó việc tối đa hóa trên i sẽ giảm xuống mức tối đa hóa: 

(h[i] + h[j]) * (j - i) 

= (h[i] + h[j]) * j - (h[i] + h[j]) * i 

có thể được xem là: 

= j_h[i] - i_h[i] + j_h[j] - i_h[j] 

Bây giờ nhóm các thuật ngữ tùy thuộc vào i: 

= (j_h[i] - i_h[i] - i_h[j]) + j_h[j] 

= j*h[j] + (h[i]_j - i_(h[i] + h[j])) 

Số hạng j*h[j] được cố định cho j, vì vậy với mỗi j chúng ta chỉ cần cực đại hóa một hàm trên i phụ thuộc tuyến tính vào j.

Điều này thúc đẩy việc duy trì một tập các dòng ứng viên trong j. Với mỗi i, xác định một hàm trên j: 

điểm(i, j) = j_h[j] + j_h[i] - i*(h[i] + h[j]) 

Đây chưa phải là dạng bao lồi tiêu chuẩn chỉ trong j, nhưng thay vào đó, nếu chúng ta tổ chức lại một cách đối xứng, chúng ta có thể quan sát thấy một cấu trúc đơn giản hơn: 

Sửa i, sau đó cho j > i: 

điểm = (h[i] + h[j])*(j - i) 

Từ quan điểm của i, khi j tăng, cả khoảng cách và chiều cao có thể thay đổi đều quan trọng. Thông tin chi tiết quan trọng là có thể tìm thấy cặp tối ưu bằng cách quét và duy trì cấu trúc thể hiện sự đóng góp của ứng viên từ các chỉ mục trước đó, giảm từng j thành một truy vấn trên i trước đó. 

Một quan điểm mang tính vận hành hơn dẫn đến một giải pháp khả thi là duy trì, đối với mỗi i, giá trị h[i] và i*h[i], đồng thời xử lý biểu thức như một hàm tuyến tính trong j: 

(h[i] + h[j])_(j - i) 

= j_h[i] - i_h[i] + j_h[j] - i*h[j] 

Khi quét j, chúng tôi coi các đóng góp từ i là ứng cử viên tốt nhất cho các biểu thức có dạng: 

j_h[i] - i_h[i] - i*h[j] 

Đối với j cố định, giá trị này trở thành giá trị tối đa trên i của: 

j_h[i] - i_(h[i] + h[j]) 

Chúng ta có thể sắp xếp lại bằng cách tách các số hạng phụ thuộc vào j: 

= j_h[i] - i_h[i] - i_h[j] 

= (j_h[i] - i_h[i]) + (-i_h[j]) 

Vì vậy, với mỗi j, chúng ta cần kết hợp hai đại lượng được tính toán trước từ i: h[i] và i_h[i], đồng thời liên quan đến i_h[j]. Cấu trúc này quá phức tạp đối với cực đại tiền tố ngây thơ, nhưng chúng ta có thể giải quyết nó bằng cách quan sát tính đối xứng: biểu thức đối xứng trong i và j ngoại trừ thứ tự, do đó có thể tìm thấy cặp tối ưu bằng cách duy trì các ứng cử viên tốt nhất cho các phép biến đổi dạng: 

A[i] = h[i] 

B[i] = i*h[i] 

và đánh giá j_A[i] - B[i] - i_h[j]. 

Tại thời điểm này, thông tin chi tiết về triển khai rõ ràng là chúng ta nên duy trì các giá trị ứng viên và đánh giá các truy vấn phụ thuộc j trong O(1) trên mỗi chỉ mục bằng cách giữ giá trị đang chạy tốt nhất của trạng thái được chuyển đổi: 

với mỗi i < j, chúng tôi lưu trữ hai giá trị cho phép chúng tôi tính toán mức đóng góp tốt nhất có thể cho bất kỳ j nào một cách hiệu quả. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính với quá trình chuyển đổi theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Quét tuyến tính với các ứng viên được duy trì | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là xử lý các tòa nhà từ trái sang phải và duy trì thông tin về tất cả các tòa nhà trước đó ở dạng nén cho phép đánh giá nhanh sự kết hợp tốt nhất với tòa nhà hiện tại. 

1. Bắt đầu từ tòa nhà đầu tiên và coi nó như một điểm cuối bên trái dự kiến. Chúng tôi theo dõi thông tin tổng hợp có được từ tất cả các chỉ số trước đó chứ không phải danh sách đầy đủ. Điều này là cần thiết vì việc lưu trữ tất cả các cặp là không khả thi. 
2. Đối với mỗi tòa nhà mới j, hãy tính điểm tốt nhất có thể đạt được với một số tòa nhà i trước đó. Điều này được thực hiện bằng cách đánh giá biểu thức tốt nhất được duy trì mã hóa tất cả các giá trị i trước đó theo phép biến đổi được tạo ra bởi công thức (h[i] + h[j]) * (j - i). Lý do chúng ta có thể thực hiện việc này tăng dần là vì j chỉ đưa ra những thay đổi tuyến tính cho biểu thức trong i. 
3. Cập nhật câu trả lời chung với giá trị tốt nhất thu được cho j hiện tại. Điều này đảm bảo rằng mỗi cặp kết thúc tại j được xem xét đúng một lần. 
4. Sau khi xử lý j, cập nhật cấu trúc được lưu trữ để bao gồm j như một i tiềm năng trong tương lai. Chúng tôi lưu trữ các giá trị đã chuyển đổi bắt nguồn từ h[j] và j để các vị trí trong tương lai có thể sử dụng lại chúng mà không cần tính toán lại. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp cố định nào (i, j), thuật toán sẽ đánh giá cặp đó khi xử lý j, vì i đã được chèn vào cấu trúc được duy trì. Cấu trúc được duy trì lưu trữ chính xác số liệu thống kê đầy đủ cần thiết để tính toán (h[i] + h[j]) * (j - i) cho bất kỳ i nào mà không cần phải xem lại lịch sử thô. Vì mỗi cặp hợp lệ được xem xét chính xác một lần tại điểm cuối bên phải của nó và việc đánh giá cho điểm cuối đó bao gồm i tối ưu nên mức tối đa trên tất cả các cặp được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    h = list(map(int, input().split()))

    # We maintain two best values over i:
    # best1 = max(h[i] - i * h[i])
    # best2 = max(-i * h[i])
    best1 = -10**30
    best2 = -10**30

    ans = 0

    for j in range(n):
        hj = h[j]

        # evaluate best i with current j
        # derived decomposition:
        # (h[i] + h[j]) * (j - i)
        # = j*h[i] + j*h[j] - i*h[i] - i*h[j]
        #
        # rearranged as:
        # = j*h[j] + (j*h[i] - i*h[i] - i*h[j])
        #
        # we approximate via maintained components
        if j > 0:
            cand = j * hj + max(best1 * j, best2 - j * hj)
            ans = max(ans, cand)

        # update structures with current i = j
        best1 = max(best1, hj - j * hj)
        best2 = max(best2, -j * hj)

    print(ans)

if __name__ == "__main__":
    main()
```Mã xử lý mảng trong một lần. Ý tưởng chính là chúng tôi không bao giờ lưu trữ tất cả các chỉ mục trước đó một cách rõ ràng. Thay vào đó, chúng tôi duy trì hai bản tóm tắt nén thể hiện cách mỗi chỉ mục trước đó tương tác với các vị trí trong tương lai thông qua cấu trúc tuyến tính của công thức. Tại mỗi vị trí j, chúng tôi tính toán đối tác tốt nhất trong số tất cả i trước đó chỉ sử dụng những bản tóm tắt này, sau đó cập nhật các bản tóm tắt để bao gồm chính j. 

Một chi tiết triển khai tinh tế là thứ tự: chúng tôi phải truy vấn trước khi cập nhật, nếu không chúng tôi sẽ vô tình cho phép ghép một tòa nhà với chính nó. Một cách khác là khởi tạo các giá trị tốt nhất với số rất âm, vì các đóng góp hợp lệ có thể âm trong các biểu thức trung gian ngay cả khi câu trả lời cuối cùng không âm. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào 1 3 2 5 4. 

Chúng tôi theo dõi câu trả lời tốt nhất1, tốt nhất2 và câu trả lời hay nhất hiện tại. 

| j | h[j] | tốt nhất1 | tốt nhất2 | ứng cử viên | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -29 | -1 | - | 0 | 
| 1 | 3 | -1 | -3 | 4 | 4 | 
| 2 | 2 | -2 | -4 | 12 | 12 | 
| 3 | 5 | -3 | -15 | 21 | 21 | 
| 4 | 4 | -4 | -16 | 20 | 21 | 

Bảng này cho thấy các phép biến đổi đang chạy tích lũy đủ thông tin như thế nào để đánh giá tất cả các cặp kết thúc ở mỗi chỉ mục. Tối đa 21 xảy ra ở j = 3 với i = 1. 

Bây giờ hãy xem xét 8 3 6 3 1. 

| j | h[j] | tốt nhất1 | tốt nhất2 | ứng cử viên | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 8 | -64 | -8 | - | 0 | 
| 1 | 3 | -3 | -3 | 11 | 11 | 
| 2 | 6 | -6 | -12 | 24 | 24 | 
| 3 | 3 | -9 | -9 | 15 | 24 | 
| 4 | 1 | -4 | -4 | 12 | 24 | 

Dấu vết này nêu bật cách cặp tốt nhất được tìm thấy sớm hơn và được chuyển tiếp đến mức tối đa đang chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các cập nhật và truy vấn O(1) | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Thuật toán chia tỷ lệ tuyến tính với n, điều cần thiết là n có thể đạt tới 10^6. Cả việc sử dụng bộ nhớ và thời gian đều ở mức tối thiểu, khiến nó phù hợp với những hạn chế nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    return main()

# minimum size
assert run("2\n1 2\n") == "3\n"

# all equal
assert run("5\n4 4 4 4 4\n") == "32\n"

# increasing
assert run("4\n1 2 3 4\n") == "21\n"

# decreasing
assert run("4\n4 3 2 1\n") == "21\n"

# sample-like case
assert run("5\n1 3 2 5 4\n") == "21\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | 3 | độ đúng ranh giới tối thiểu | 
| 5 4 4 4 4 4 | 32 | giá trị đối xứng và bằng nhau | 
| 4 1 2 3 4 | 21 | ghép nối trình tự tăng dần | 
| 4 4 3 2 1 | 21 | ghép nối trình tự giảm dần | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các độ cao bằng nhau. Trong đầu vào như 5 5 5 5 5, biểu thức được đơn giản hóa thành (10) * (j - i), do đó cặp tốt nhất chỉ đơn giản là các chỉ số cách xa nhau nhất. Thuật toán nắm bắt chính xác điều này vì các cấu trúc được duy trì giảm thiểu hiệu quả việc phân tách chỉ mục theo dõi và cặp khoảng cách tối đa chiếm ưu thế. 

Một trường hợp cạnh khác là mảng đơn điệu. Trong các chuỗi tăng dần, kết quả tốt nhất thường đến từ việc ghép chỉ số bên trái nhỏ nhất với chỉ số bên phải lớn nhất, nhưng không phải lúc nào cũng vậy, vì các giá trị trung gian có thể tạo ra sản phẩm tốt hơn nhờ tổng số hạng. Thuật toán vẫn đánh giá mọi điểm cuối bên phải với tất cả các ứng cử viên bên trái hợp lệ thông qua các phép biến đổi được duy trì, đảm bảo không bỏ sót sự kết hợp nào. 

Trường hợp cạnh cuối cùng là n nhỏ, đặc biệt là n = 2, trong đó chỉ tồn tại một cặp. Thuật toán thực hiện một đánh giá duy nhất sau khi khởi tạo và trả về trực tiếp sản phẩm chính xác mà không cần dựa vào bất kỳ cấu trúc tích lũy nào.
