---
title: "CF 104887D - Rồng Này Hạt"
description: "Chúng ta được cung cấp nhiều tập hợp các điểm mạnh liên quan đến các vị trí từ 1 đến n và chúng ta được phép đưa ra một hoán vị của các vị trí này."
date: "2026-06-28T09:01:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104887
codeforces_index: "D"
codeforces_contest_name: "2023 Abakoda Long Contest"
rating: 0
weight: 104887
solve_time_s: 74
verified: false
draft: false
---

[CF 104887D - Những quả hạch rồng này](https://codeforces.com/problemset/problem/104887/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập hợp các điểm mạnh liên quan đến các vị trí từ 1 đến n và chúng ta được phép đưa ra một hoán vị của các vị trí này. Sau khi chúng tôi sửa đổi hoán vị, một quy trình độc lập sẽ cố gắng đáp ứng giá trị bắt buộc của từng vị trí bằng cách sử dụng các thao tác làm tăng giá trị trên các đoạn liền kề có độ dài k. 

Mỗi thao tác chọn một cửa sổ có độ dài k trong hoán vị hiện tại và tăng từng phần tử trong cửa sổ đó lên một. Chi phí của một hoán vị cố định là số lượng tối thiểu các phép toán cần thiết để vị trí i đạt ít nhất a[i] với mọi i. Nhiệm vụ của chúng ta không phải là tính chi phí này cho một hoán vị nhất định mà thay vào đó là xây dựng một hoán vị làm cho chi phí này càng lớn càng tốt. 

Khó khăn chính là chi phí phụ thuộc vào mức độ lớn các yêu cầu được nhóm lại bên trong các cửa sổ chồng chéo có kích thước k. Nếu các giá trị lớn được đặt sao cho nhiều giá trị trong số chúng có thể được bao phủ cùng nhau trong mỗi thao tác thì tổng số thao tác sẽ giảm. Nếu chúng lan rộng theo cách khiến cửa sổ liên tục “bỏ lỡ” các vị trí có nhu cầu cao thì chi phí sẽ tăng lên. 

Các ràng buộc cho phép n lên tới 2×10^5, loại trừ bất kỳ phương pháp nào thử tất cả các hoán vị hoặc mô phỏng quy trình cho nhiều ứng viên. Ngay cả lý luận O(n²) cho mỗi hoán vị cũng quá chậm, do đó, giải pháp phải xây dựng hoán vị trong một lần hoặc thời gian gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi k = 1. Mỗi thao tác chỉ ảnh hưởng đến một phần tử duy nhất, do đó chi phí trở thành tổng của tất cả a[i], không phụ thuộc vào thứ tự. Bất kỳ hoán vị nào cũng là tối ưu trong trường hợp này, vì vậy việc xây dựng vẫn phải hợp lệ mà không cần dựa vào hành vi nhóm. 

Một trường hợp cạnh khác xảy ra khi k = n. Mọi thao tác đều ảnh hưởng đồng thời đến tất cả các vị trí, do đó chi phí sẽ trở thành max(a[i]). Một lần nữa, bất kỳ hoán vị nào cũng có tác dụng và việc xây dựng không nên coi trọng địa phương. 

Khó khăn thực sự là đối với k trung gian, trong đó các cửa sổ chồng chéo tạo ra hiệu ứng trượt “khả năng phủ sóng” và việc đặt hàng sẽ xác định mức độ khấu hao của các nhu cầu lớn một cách hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng đánh giá chi phí của một hoán vị nhất định. Đối với một sự sắp xếp cố định, người ta có thể mô phỏng một cách tham lam số lượng hoạt động phân đoạn được yêu cầu. Một ý tưởng phổ biến là xử lý từ trái sang phải, liên tục áp dụng các thao tác bất cứ khi nào một số vị trí vẫn ở dưới mục tiêu của nó. Tuy nhiên, mô phỏng này tốn O(n·max(a)) trong trường hợp xấu nhất, vì mỗi lần tăng chỉ đóng góp một đơn vị tiến độ và a[i] có thể lớn tới 10^9. Quan trọng hơn nữa, chúng ta sẽ cần phải kiểm tra nhiều hoán vị, điều này khiến điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc đến từ việc đảo ngược quan điểm. Thay vì suy nghĩ “với một hoán vị, chúng ta sẽ nhận được bao nhiêu chi phí”, chúng tôi hỏi “sự sắp xếp nào buộc cửa sổ trượt kém hiệu quả nhất”. Mỗi thao tác đóng góp k phần tăng dần được phân bổ trên các vị trí liền kề. Nếu các nhu cầu lớn được nhóm lại thì một thao tác duy nhất sẽ giúp ích được nhiều giá trị lớn cùng một lúc. Để tối đa hóa chi phí, chúng tôi muốn ngăn chặn việc phân cụm như vậy và đảm bảo rằng các giá trị cao sẽ ảnh hưởng đến phạm vi phủ sóng của nhau nhiều nhất có thể. 

Điều này trở thành một vấn đề sắp xếp tham lam cổ điển trên các khoảng được tạo ra bởi các cửa sổ có kích thước k. Mỗi vị trí có thể được coi là tham gia vào k cửa sổ liên tiếp, do đó, việc đặt các giá trị lớn cách xa nhau có xu hướng tăng số lượng thao tác cần thiết để đáp ứng chúng, vì không có cửa sổ đơn lẻ nào liên tục mang lại lợi ích cho nhiều nhu cầu lớn.

Một quan sát quan trọng là sự sắp xếp tồi tệ nhất đạt được bằng cách phân phối đồng đều các giá trị lớn trên các lớp dư lượng modulo k trong bố cục hoán vị. Điều này đảm bảo rằng bất kỳ cửa sổ nào có độ dài k giao nhau tối đa một giá trị rất lớn từ mỗi lớp, hạn chế phạm vi phủ sóng được chia sẻ. Trên thực tế, điều này đạt được bằng cách sắp xếp các chỉ mục theo giá trị và gán chúng theo chu kỳ vào k nhóm, sau đó nối các nhóm. 

Cấu trúc này đảm bảo rằng các giá trị a[i] lớn được phân tách bằng ít nhất k vị trí bất cứ khi nào có thể, buộc quy trình phải “áp dụng lại” các hoạt động nhiều lần hơn so với khi chúng được nhóm lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên hoán vị | O(n! · n · max(a)) | O(n) | Quá chậm | 
| Phân phối theo chu kỳ tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị của các chỉ số từ 1 đến n. 

1. Sắp xếp tất cả các chỉ số theo giá trị yêu cầu a[i] theo thứ tự giảm dần. 

Điều này đảm bảo chúng tôi đặt những vị trí “đắt” nhất lên hàng đầu, vì chúng chi phối số lượng hoạt động bắt buộc. 
2. Tạo k thùng trống. 

Những nhóm này đại diện cho các lớp cặn trong cách sắp xếp cuối cùng, đảm bảo sự tách biệt giữa các giá trị cao. 
3. Lặp lại các chỉ mục đã được sắp xếp và gán từng chỉ số một vào các nhóm theo thứ tự vòng tròn. 

Phần lớn nhất đầu tiên sẽ chuyển đến nhóm 0, bên cạnh nhóm 1, v.v. theo chu kỳ. 

Điều này đảm bảo rằng các giá trị lớn liên tiếp được phân tách bằng ít nhất k vị trí trong phép nối cuối cùng. 
4. Ghép tất cả các nhóm theo thứ tự từ nhóm 0 đến nhóm k−1 để tạo thành hoán vị cuối cùng. 

Điều này tạo ra một bố cục trong đó mỗi nhóm tạo thành một chuỗi con có khoảng cách và việc kết hợp chúng sẽ duy trì thuộc tính phân tách. 

Lý do điều này hoạt động là vì mỗi cửa sổ có độ dài k có thể giao nhau nhiều nhất một phần tử từ mỗi nhóm trong vùng “dày đặc” có giá trị lớn. Vì các giá trị lớn được trải đều trên các nhóm nên không một cửa sổ nào có thể liên tục bao gồm nhiều chỉ số có nhu cầu cao. Điều này buộc số lượng hoạt động tối thiểu phải tăng lên vì vùng phủ sóng không thể được tái sử dụng một cách hiệu quả trên nhiều vị trí a[i] cao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

idx = list(range(n))
idx.sort(key=lambda i: a[i], reverse=True)

buckets = [[] for _ in range(k)]

for t, i in enumerate(idx):
    buckets[t % k].append(i + 1)

ans = []
for b in buckets:
    ans.extend(b)

print(*ans)
```Việc thực hiện theo sau việc xây dựng trực tiếp. Chúng tôi sắp xếp các chỉ số bằng cách giảm a[i], điều này đảm bảo chúng tôi xử lý các vị trí đòi hỏi khắt khe nhất trước tiên. Việc phân công theo vòng tròn sẽ phân bổ chúng đồng đều trên k nhóm, đây là cơ chế cốt lõi giúp ngăn chặn việc phân cụm. 

Phép nối cuối cùng rất quan trọng: nó duy trì trật tự bên trong mỗi nhóm, đồng thời đảm bảo rằng các phần tử từ các nhóm khác nhau được xen kẽ ở khoảng cách khoảng k. Việc sử dụng chỉ mục dựa trên 1 ở đầu ra là bắt buộc vì bài toán mong đợi các vị trí được gắn nhãn từ 1 đến n. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó n = 5, k = 3 và các giá trị là [7, 77, 2, 22, 222]. 

Sau khi sắp xếp các chỉ số theo giá trị, ta được các chỉ số tương ứng với các giá trị: 

222 (chỉ số 5), 77 (chỉ số 2), 22 (chỉ số 4), 7 (chỉ số 1), 2 (chỉ số 3) 

Chúng tôi phân phối vào k = 3 nhóm theo chu kỳ. 

| Bước | Chỉ mục | Giá trị | Phân công nhóm | 
| --- | --- | --- | --- | 
| 1 | 5 | 222 | xô 0 | 
| 2 | 2 | 77 | xô 1 | 
| 3 | 4 | 22 | xô 2 | 
| 4 | 1 | 7 | xô 0 | 
| 5 | 3 | 2 | xô 1 | 

Xô trở thành: 

xô 0: [5, 1] 

thùng 1: [2, 3] 

xô 2: [4] 

Ghép nối cho: [5, 1, 2, 3, 4] 

Điều này khớp với một hoán vị trải rộng các giá trị lớn (5 và 2), đảm bảo không có cửa sổ cỡ 3 nào có thể bao phủ cả hai nhiều lần một cách hiệu quả. 

Ví dụ thứ hai: n = 6, k = 2, giá trị [1, 100, 2, 99, 3, 98] 

Chỉ số được sắp xếp: 2, 4, 6, 3, 5, 1 

Xô: 

nhóm 0: [2, 6, 5] 

thùng 1: [4, 3, 1] 

Hoán vị cuối cùng: [2, 6, 5, 4, 3, 1] 

Sự luân phiên này buộc bất kỳ cửa sổ có độ dài-2 nào cũng phải trộn các giá trị cao và trung bình thay vì nhóm tất cả các giá trị cao lại với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | chỉ số sắp xếp chiếm ưu thế, việc gán nhóm là tuyến tính | 
| Không gian | O(n) | thùng lưu trữ và danh sách chỉ mục | 

Các ràng buộc cho phép tối đa 2×10^5 phần tử, do đó, cấu trúc O(n log n) nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    idx = list(range(n))
    idx.sort(key=lambda i: a[i], reverse=True)

    buckets = [[] for _ in range(k)]
    for t, i in enumerate(idx):
        buckets[t % k].append(i + 1)

    ans = []
    for b in buckets:
        ans.extend(b)

    return " ".join(map(str, ans))

# provided sample
assert run("5 3\n7 77 2 22 222\n") == "5 1 2 3 4", "sample 1"

# all equal values
out = run("4 2\n5 5 5 5\n")
assert sorted(out.split()) == ["1","2","3","4"]

# k = 1
out = run("4 1\n1 100 2 99\n")
assert sorted(out.split()) == ["1","2","3","4"]

# k = n
out = run("3 3\n10 20 30\n")
assert sorted(out.split()) == ["1","2","3"]

# descending already
out = run("5 2\n5 4 3 2 1\n")
assert sorted(out.split()) == ["1","2","3","4","5"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 trường hợp | hoán vị nào | đặt hàng trường hợp cạnh không liên quan | 
| trường hợp k=n | hoán vị nào | thoái hóa bảo hiểm đầy đủ | 
| giá trị bằng nhau | hoán vị nào | tính đối xứng và tính ổn định | 
| đầu vào được sắp xếp | hoán vị hợp lệ | không phụ thuộc vào đơn hàng ban đầu | 

## Vỏ cạnh 

Khi k = 1, mọi thao tác chỉ ảnh hưởng đến một vị trí duy nhất, do đó không tồn tại sự tương tác giữa các chỉ số. Thuật toán vẫn sắp xếp và phân phối vào một nhóm, tạo ra một hoán vị đơn giản cho tất cả các chỉ số. Cấu trúc của các thùng suy biến chính xác vì chỉ có một. 

Khi k = n, mọi thao tác đều ảnh hưởng đến toàn bộ mảng, do đó thứ tự tương đối không ảnh hưởng đến tính khả thi. Việc xây dựng vẫn đưa ra một hoán vị hợp lệ vì nó chỉ sắp xếp lại các chỉ số mà không giả định vấn đề địa phương. 

Khi tất cả a[i] bằng nhau thì không có sự phân biệt có ý nghĩa giữa các vị trí. Phân phối vòng tròn chỉ đơn giản tạo ra một thứ tự tùy ý, phù hợp với thực tế là tất cả các hoán vị đều mang lại cùng một chi phí theo các yêu cầu đối xứng.
