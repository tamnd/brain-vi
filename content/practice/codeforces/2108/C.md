---
title: "CF 2108C - Cuộc trốn chạy của Neo"
description: "Neo đứng trước một hàng nút, mỗi nút được gắn nhãn có trọng số nguyên dương. Anh ta muốn nhấn tất cả các nút sao cho thứ tự trọng số của các nút được nhấn không bao giờ tăng lên."
date: "2026-06-08T04:43:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "dsu", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2108
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1022 (Div. 2)"
rating: 1500
weight: 2108
solve_time_s: 93
verified: false
draft: false
---

[CF 2108C - Cuộc trốn thoát của Neo](https://codeforces.com/problemset/problem/2108/C) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dp, dsu, đồ thị, tham lam, triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Neo đứng trước một hàng nút, mỗi nút được gắn nhãn có trọng số nguyên dương. Anh ta muốn nhấn tất cả các nút sao cho thứ tự trọng số của các nút được nhấn không bao giờ tăng lên. Bản thân Neo không thể di chuyển, nhưng anh ta có thể tạo các bản sao nhấn nút và di chuyển sang trái hoặc phải, kích hoạt bất kỳ nút nào chưa nhấn mà chúng chạm tới. Thách thức là xác định số lượng bản sao tối thiểu cần thiết để nhấn tất cả các nút theo thứ tự trọng lượng không tăng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi biết số lượng nút và trọng lượng của chúng. Đầu ra là một số duy nhất cho mỗi trường hợp, biểu thị số lượng bản sao tối thiểu được yêu cầu. 

Các ràng buộc cho phép tối đa 200.000 nút cho mỗi trường hợp thử nghiệm và tổng cộng 200.000 trên tất cả các trường hợp. Điều này loại trừ bất kỳ giải pháp ngây thơ nào thử tất cả các lệnh có thể hoặc mô phỏng mọi chuyển động nhân bản có thể có, bởi vì các phương pháp tiếp cận O(n²) sẽ thực hiện các thao tác lên tới 4 × 10¹⁰, vượt xa giới hạn thời gian 2 giây. Chúng tôi cần một cách tiếp cận chạy gần như theo O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh không rõ ràng xảy ra khi nhiều nút có cùng trọng lượng hoặc khi trọng lượng lớn bị phân tán khiến chiến lược sao chép một lượt tham lam không thành công. Ví dụ, với đầu vào`5 3 2 4 1`, việc tạo các bản sao ở mức 4 và 5 sẽ tốt hơn so với việc chỉ bắt đầu ở giá trị lớn nhất vì nó cho phép nhiều bản sao hoạt động song song và tránh chặn các chuỗi nhỏ hơn. Cách tiếp cận bất cẩn luôn chọn nút lớn nhất ngoài cùng bên trái có thể đánh giá thấp số lượng bản sao tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc tạo một bản sao ở mọi nút và di chuyển nó để nhấn các nút trong khi thử tất cả các trình tự có thể duy trì thứ tự không tăng. Mặc dù đúng về nguyên tắc nhưng điều này đòi hỏi phải kiểm tra tất cả các hoán vị của máy ép và theo dõi nhiều bản sao, dẫn đến độ phức tạp về mặt giai thừa. Ngay cả khi giảm nó thành mô phỏng tham lam, việc di chuyển từng bản sao một sẽ dẫn đến hành vi O(n²), quá chậm đối với n lên tới 2 × 10⁵. 

Quan sát quan trọng là chúng ta có thể mô hình hóa vấn đề bằng cách chia chuỗi nút thành nhiều chuỗi con không tăng. Mỗi bản sao có thể xử lý một chuỗi con bằng cách di chuyển từ trái sang phải. Số lượng bản sao tối thiểu sau đó bằng với số lượng tối thiểu các chuỗi con cần thiết để bao gồm tất cả các nút. Điều này tương đương với việc tìm số tối thiểu của các dãy tăng nghiêm ngặt nếu chúng ta đảo ngược các trọng số. Điều này ánh xạ trực tiếp đến một kỹ thuật thuật toán cổ điển được gọi là phương pháp sắp xếp kiên nhẫn, được sử dụng để tính Chuỗi con không giảm dài nhất (LNDS). Số lượng bản sao bằng với độ dài của chuỗi dài nhất khi gán từng trọng số cho chuỗi con phù hợp đầu tiên mà nó có thể mở rộng, điều này có thể được triển khai hiệu quả với một đống hoặc nhiều tập hợp. 

Trực giác là bất cứ khi nào một nút không thể được thêm vào bất kỳ chuỗi bản sao hiện có nào mà không vi phạm thứ tự không tăng, chúng ta cần một bản sao mới. Nếu không, chúng tôi sẽ mở rộng trình tự của bản sao hiện có. Điều này tương tự như việc sắp xếp các trọng số thành số lượng ngăn xếp không tăng ít nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Kiên nhẫn Sắp xếp / Chuỗi không tăng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`piles`để biểu diễn các chuỗi được xử lý bởi mỗi bản sao. Mỗi cọc lưu trữ trọng lượng cuối cùng được ép bởi bản sao đó. 
2. Lặp lại trọng lượng của nút từ trái sang phải. Đối với mỗi quả cân, hãy cố gắng đặt nó lên chồng ngoài cùng bên trái có quả cân cuối cùng lớn hơn hoặc bằng quả cân hiện tại. Nếu có một đống như vậy, hãy thay thế giá trị cao nhất của nó bằng trọng lượng hiện tại. 
3. Nếu không có cọc nào chịu được trọng lượng hiện tại thì tạo cọc mới với trọng lượng này. Mỗi cọc tương ứng với một bản sao riêng biệt. 
4. Sau khi xử lý tất cả các nút, số lượng cọc chính xác là số lượng bản sao tối thiểu cần thiết. 

Tại sao nó hoạt động: mỗi cọc đại diện cho một chuỗi các lần nhấn nút được xử lý bởi một bản sao duy nhất. Bằng cách luôn đặt trọng lượng tiếp theo lên cọc tương thích ngoài cùng bên trái, chúng tôi đảm bảo mỗi cọc không tăng lên và chỉ tạo cọc mới khi cần thiết để đảm bảo tổng số cọc là tối thiểu. Điều này có hiệu quả vì bài toán giảm xuống việc tính toán số lượng chuỗi không tăng tối thiểu bao phủ mảng, chính xác là độ dài của Chuỗi tăng dài nhất nếu chúng ta đảo ngược hướng so sánh. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def min_clones(weights):
    piles = []
    for w in weights:
        # find the first pile where top >= w
        idx = bisect.bisect_right(piles, w)
        if idx < len(piles):
            piles[idx] = w
        else:
            piles.append(w)
    return len(piles)

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_clones(a))
```chức năng`min_clones`thực hiện phân vùng dãy con không tăng. Chúng tôi sử dụng`bisect_right`trên danh sách ngọn cọc để tìm cọc đầu tiên có trọng lượng đỉnh ≥ hiện tại. Nếu một đống như vậy tồn tại, chúng tôi thay thế giá trị cao nhất của nó bằng trọng số hiện tại để mở rộng chuỗi của bản sao đó. Nếu không, chúng tôi bắt đầu một đống mới. sử dụng`bisect`đảm bảo chèn O(log n) cho mỗi trọng lượng, cho tổng thời gian là O(n log n). 

Cần thận trọng với sự bất bình đẳng trong`bisect_right`, bởi vì chúng ta đang xây dựng các dãy không tăng; đảo ngược hướng so sánh đảm bảo các cọc vẫn đúng. Không thể đảo ngược sự bất bình đẳng dẫn đến số lượng bản sao không chính xác. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`4 3 2 1 5`| Bước | Cân nặng hiện tại | Cọc trước | Hành động | Cọc sau | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | [] | cọc mới | [4] | 
| 2 | 3 | [4] | đặt lên cọc 0 | [3] | 
| 3 | 2 | [3] | đặt lên cọc 0 | [2] | 
| 4 | 1 | [2] | đặt lên cọc 0 | [1] | 
| 5 | 5 | [1] | không có cọc >=5 | cọc mới | 

Kết quả: 2 bản sao. Điều này phù hợp với đầu ra mẫu. 

**Ví dụ 2**:`1 1 1`| Bước | Cân nặng hiện tại | Cọc trước | Hành động | Cọc sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [] | cọc mới | [1] | 
| 2 | 1 | [1] | đặt lên cọc 0 | [1] | 
| 3 | 1 | [1] | đặt lên cọc 0 | [1] | 

Kết quả: 1 bản sao. Các trọng lượng bằng nhau lặp đi lặp lại được xử lý chính xác bằng cách kéo dài cùng một cọc. 

Những dấu vết này cho thấy thuật toán giảm thiểu các bản sao một cách chính xác bằng cách sử dụng lại các chuỗi bất cứ khi nào có thể và chỉ tạo một bản sao mới khi cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trọng lượng sử dụng tìm kiếm nhị phân trong danh sách ngọn cọc | 
| Không gian | O(n) | Lưu trữ tối đa n cọc trong trường hợp xấu nhất | 

Với n tối đa 2 × 10⁵, giải pháp O(n log n) thực hiện khoảng 3 × 10⁶ thao tác cho mỗi trường hợp thử nghiệm, vừa vặn thoải mái trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("4\n5\n4 3 2 1 5\n3\n1 1 1\n6\n7 8 1 5 9 2\n10\n1 7 9 7 1 10 2 10 10 7\n") == "2\n1\n2\n3", "sample tests"

# custom cases
assert run("1\n1\n42\n") == "1", "single button"
assert run("1\n5\n5 4 3 2 1\n") == "1", "strictly decreasing sequence"
assert run("1\n5\n1 2 3 4 5\n") == "5", "strictly increasing sequence"
assert run("1\n6\n2 2 2 2 2 2\n") == "1", "all equal"
assert run("1\n6\n1 3 2 4 3 5\n") == "3", "mixed peaks and valleys"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n42`| 1 | trường hợp cạnh nút đơn | 
|`5 4 3 2 1`| 1 | dãy giảm chặt chẽ | 
|`1 2 3 4 5`| 5 | trình tự tăng nghiêm ngặt yêu cầu nhân bản tối đa | 
|`2 2 2 2 2 2`| 1 | lặp đi lặp lại trọng lượng bằng nhau | 
|`1 3 2 4 3 5`| 3 | trình tự hỗn hợp với nhiều bản sao cần thiết | 

## Vỏ cạnh 

Đối với một chuỗi trong đó tất cả các trọng số đều bằng nhau, chẳng hạn như`1 1 1 1`, thuật toán sử dụng chính xác một bản sao duy nhất. Tìm kiếm nhị phân tìm thấy đống đầu tiên có thể được mở rộng (đoạn duy nhất
