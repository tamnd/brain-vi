---
title: "CF 976C - Phân đoạn lồng nhau"
description: "Chúng ta có một tập hợp các khoảng đóng trên trục số. Mỗi khoảng đại diện cho một phân đoạn có điểm cuối bên trái và điểm cuối bên phải và chúng ta cần xác định xem liệu có tồn tại một cặp phân đoạn riêng biệt sao cho một phân đoạn được chứa hoàn toàn bên trong phân đoạn kia hay không."
date: "2026-06-17T01:29:46+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 976
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 43 (Rated for Div. 2)"
rating: 1500
weight: 976
solve_time_s: 76
verified: true
draft: false
---

[CF 976C - Phân đoạn lồng nhau](https://codeforces.com/problemset/problem/976/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các khoảng đóng trên trục số. Mỗi khoảng đại diện cho một phân đoạn có điểm cuối bên trái và điểm cuối bên phải và chúng ta cần xác định xem liệu có tồn tại một cặp phân đoạn riêng biệt sao cho một phân đoạn được chứa hoàn toàn bên trong phân đoạn kia hay không. 

Về mặt hình thức, đối với hai phân đoạn$[l_i, r_i]$Và$[l_j, r_j]$, chúng tôi muốn tìm chỉ số$i \neq j$như vậy$l_i \ge l_j$Và$r_i \le r_j$. Nếu một cặp như vậy tồn tại, chúng tôi trả về các chỉ mục của bất kỳ cặp lồng nhau hợp lệ nào; nếu không thì chúng tôi báo cáo rằng không có sự lồng nhau nào tồn tại. 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$các phân đoạn và điểm cuối có thể lớn bằng$10^9$. Điều này ngay lập tức loại trừ bất kỳ chiến lược so sánh bậc hai nào. Một sự ngây thơ$O(n^2)$quá trình quét sẽ thực hiện khoảng$9 \cdot 10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể thực hiện trong hai giây. 

Khó khăn chính là việc ngăn chặn phụ thuộc vào hai chiều cùng một lúc. Việc sắp xếp chỉ theo một tọa độ không tự động bảo toàn tọa độ kia, vì vậy việc sắp xếp bất cẩn có thể ẩn các câu trả lời hợp lệ. 

Một số tình huống khó khăn minh họa những cạm bẫy phổ biến. Nếu tất cả các phân đoạn đều giống hệt nhau thì bất kỳ cặp nào cũng hợp lệ và phải được phát hiện; một giải pháp chỉ tìm kiếm sự bất bình đẳng nghiêm ngặt có thể bỏ lỡ điều này một cách không chính xác. Nếu các phân đoạn đang tăng nghiêm ngặt ở cả hai điểm cuối như$[1,2], [2,3], [3,4]$, không có sự ngăn chặn mặc dù các khoảng chồng lên nhau một cách có cấu trúc. Một trường hợp phức tạp khác là khi một đoạn rất dài và chứa nhiều đoạn khác, chẳng hạn$[1,100]$chứa đựng$[2,99]$,$[3,98]$, vân vân. Một thuật toán chỉ so sánh các hàng xóm theo thứ tự ban đầu sẽ thất bại ở đây trừ khi cấu trúc được tổ chức lại. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force kiểm tra từng cặp phân đoạn và xác minh xem phân đoạn này có chứa phân đoạn kia hay không. Điều này thật đơn giản: với mỗi$i$, so sánh nó với tất cả$j > i$và kiểm tra điều kiện ngăn chặn. Điều này đúng vì nó đánh giá rõ ràng tất cả các cặp có thể, nhưng chi phí của nó tăng theo phương trình bậc hai. Với$3 \cdot 10^5$các phân đoạn, số lượng so sánh trở nên hạn chế và thậm chí các yếu tố không đổi được tối ưu hóa cũng không thể cứu vãn được. 

Quan sát quan trọng là việc ngăn chặn sẽ trở nên dễ phát hiện hơn nhiều nếu chúng ta giảm vấn đề xuống một trật tự được lựa chọn cẩn thận. Nếu chúng ta sắp xếp các phân đoạn theo điểm cuối bên trái theo thứ tự tăng dần thì bất kỳ vùng chứa tiềm năng nào cũng phải xuất hiện sớm hơn hoặc ở cùng vị trí theo thứ tự này. Tuy nhiên, điều này vẫn chưa đủ vì trong số các đoạn có cùng điểm cuối bên trái, đoạn có điểm cuối bên phải lớn hơn sẽ được coi là đoạn bên ngoài. 

Điều này dẫn đến một cấu trúc chính xác hơn: sắp xếp theo điểm cuối bên trái tăng dần và đối với các mối quan hệ, sắp xếp theo điểm cuối bên phải giảm dần. Theo thứ tự này, bất cứ khi nào chúng tôi quét qua danh sách, bất kỳ phân đoạn nào bắt đầu sớm hơn hoặc tại cùng một điểm và kết thúc sau đều là vùng chứa tiềm năng cho tất cả các phân đoạn sau. 

Sau đó, chúng tôi duy trì phân đoạn tốt nhất (điểm cuối bên phải lớn nhất) được thấy cho đến nay trong khi quét. Nếu phân đoạn hiện tại kết thúc ở mức tối đa hoặc trước mức tối đa này thì phân đoạn đó sẽ được chứa trong phân đoạn đã xem trước đó. Nếu không, nó sẽ trở thành vùng chứa ứng cử viên mới. 

Điều này biến một bài toán so sánh hai chiều thành một bài toán duy nhất với một bất biến được duy trì. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Quét sắp xếp có theo dõi |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành việc phát hiện mối quan hệ ngăn chặn trong khi quét danh sách đã sắp xếp. 

1. Đính kèm chỉ mục vào tất cả các phân đoạn để chúng ta có thể trả về vị trí ban đầu sau khi sắp xếp. Điều này là cần thiết vì việc sắp xếp sẽ phá hủy thứ tự đầu vào. 
2. Sắp xếp các đoạn bằng cách tăng dần điểm cuối bên trái. Nếu hai đoạn có chung điểm cuối bên trái, hãy sắp xếp theo điểm cuối bên phải giảm dần. Thứ tự này đảm bảo rằng để bắt đầu bằng nhau, khoảng rộng nhất sẽ đến trước, đây là khoảng duy nhất có thể chứa những khoảng khác có cùng ranh giới bên trái. 
3. Khởi tạo hai biến: một để lưu trữ điểm cuối bên phải tối đa được thấy cho đến nay và một biến khác để lưu trữ phân đoạn nào đã đạt được mức tối đa đó. 
4. Lặp lại các phân đoạn được sắp xếp từ trái sang phải. Đối với mỗi phân đoạn$[l, r]$, so sánh điểm cuối bên phải của nó với điểm cuối bên phải tối đa hiện tại. 
5. Nếu$r \le \text{maxR}$, thì phân đoạn này được chứa đầy đủ trong phân đoạn đã tạo ra$\text{maxR}$, bởi vì đoạn đó bắt đầu không muộn hơn và kéo dài ít nhất đến tận bên phải. 
6. Nếu không tìm thấy ngăn chặn và điểm cuối bên phải của phân đoạn hiện tại lớn hơn$\text{maxR}$, cập nhật phân đoạn tốt nhất được lưu trữ vào phân đoạn này. 

Bước suy luận quan trọng là khi chúng ta đảm bảo thứ tự được sắp xếp, điều kiện duy nhất còn thiếu để ngăn chặn là so sánh điểm cuối bên phải, vì điều kiện điểm cuối bên trái đã được thực thi ngầm theo thứ tự. 

### Tại sao nó hoạt động 

Ở mỗi bước quét, phân đoạn được lưu trữ dưới dạng có điểm cuối bên phải tối đa trong số tất cả các phân đoạn đã thấy trước đó được đảm bảo là vùng chứa ứng cử viên hợp lệ cho bất kỳ phân đoạn nào trong tương lai vì nó có điểm cuối bên trái nhỏ nhất có thể có trong số những phân đoạn có phạm vi tiếp cận lớn nhất ở bên phải. Bất kỳ phân đoạn nào không vượt qua thử nghiệm ngăn chặn đều phải mở rộng sang bên phải hơn tất cả các phân đoạn trước đó, trong trường hợp đó nó sẽ trở thành ứng cử viên mới tốt nhất. Bất biến này đảm bảo rằng nếu có một cặp ngăn chặn tồn tại thì nó sẽ được phát hiện chính xác khi chúng ta gặp đoạn bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
segs = []

for i in range(n):
    l, r = map(int, input().split())
    segs.append((l, r, i + 1))

segs.sort(key=lambda x: (x[0], -x[1]))

best_r = -1
best_idx = -1

for l, r, idx in segs:
    if r <= best_r:
        print(idx, best_idx)
        sys.exit(0)
    if r > best_r:
        best_r = r
        best_idx = idx

print(-1, -1)
```Giải pháp bắt đầu bằng cách giữ nguyên các chỉ số gốc, vì việc sắp xếp thay đổi thứ tự nhưng đầu ra phải tham chiếu đến vị trí đầu vào. Bước sắp xếp là sự chuyển đổi cấu trúc giúp phát hiện biện pháp ngăn chặn chỉ trong một lần thực hiện. 

Trong quá trình quét,`best_r`đại diện cho điểm cuối bên phải xa nhất được thấy cho đến nay trong số các phân đoạn là vùng chứa tiềm năng. Nếu chúng tôi tìm thấy một phân đoạn có điểm cuối bên phải không vượt quá giá trị này thì phân đoạn đó phải được bao phủ hoàn toàn bởi phân đoạn trước đó vì các phân đoạn trước đó cũng có điểm cuối bên trái nhỏ hơn hoặc bằng phân đoạn hiện tại do sắp xếp. 

Việc thoát sớm là an toàn vì khi tìm thấy một cặp hợp lệ thì bất kỳ cặp nào cũng được chấp nhận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 10
2 9
3 9
2 3
2 9
```Thứ tự sắp xếp (l asc, r desc): 

| Bước | Phân đoạn | tốt nhất_r | tốt nhất_idx | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (1,10,1) | 10 | 1 | cập nhật | 
| 2 | (2,9,2) | 10 | 1 | 9 ≤ 10 → đáp án | 

Ở bước 2, phân đoạn 2 kết thúc bên trong phân đoạn 1, vì vậy chúng ta xuất ra một cặp hợp lệ. 

Điều này chứng tỏ phân khúc đầu tiên trở thành ứng cử viên vùng chứa phổ biến cho những phân khúc sau như thế nào. 

### Ví dụ 2 

đầu vào:```
4
1 5
2 6
3 4
4 7
```Đã sắp xếp: 

| Bước | Phân đoạn | tốt nhất_r | tốt nhất_idx | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | (1,5,1) | 5 | 1 | cập nhật | 
| 2 | (2,6,2) | 6 | 2 | cập nhật | 
| 3 | (3,4,3) | 6 | 2 | 4 6 → đáp án | 

Ở bước 3, đoạn (3,4) được chứa trong (2,6), xác nhận bất biến rằng đoạn tốt nhất cho đến nay luôn đủ để kiểm tra khả năng ngăn chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian |$O(n)$| lưu trữ các phân đoạn với các chỉ số | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$các phân đoạn, vì vậy một$O(n \log n)$cách tiếp cận cũng nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính và phù hợp thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    n = int(input())
    segs = []
    for i in range(n):
        l, r = map(int, input().split())
        segs.append((l, r, i + 1))

    segs.sort(key=lambda x: (x[0], -x[1]))

    best_r = -1
    best_idx = -1

    for l, r, idx in segs:
        if r <= best_r:
            return f"{idx} {best_idx}"
        if r > best_r:
            best_r = r
            best_idx = idx

    return "-1 -1"

# provided sample
assert run("5\n1 10\n2 9\n3 9\n2 3\n2 9\n") in ["2 1", "3 1", "4 1", "5 1", "3 2", "4 2", "5 2", "2 5", "4 5"], "sample 1"

# all identical segments
assert run("3\n1 1\n1 1\n1 1\n") != "-1 -1"

# no nesting
assert run("3\n1 2\n2 3\n3 4\n") == "-1 -1"

# clear nesting
assert run("3\n1 10\n2 5\n6 9\n") != "-1 -1"

# reverse nested chain
assert run("4\n1 8\n2 7\n3 6\n4 5\n") != "-1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các phân khúc giống hệt nhau | bất kỳ cặp hợp lệ nào | các bản sao được xử lý | 
| chuỗi ngày càng tăng | -1 -1 | không có kết quả dương tính giả | 
| làm tổ rõ ràng | cặp hợp lệ | tính đúng đắn cơ bản | 
| chuỗi sâu | cặp hợp lệ | cấu trúc lồng nhau trong trường hợp xấu nhất | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi nhiều đoạn chia sẻ cùng một điểm cuối bên trái. Ví dụ:```
3
1 10
1 5
1 7
```Sau khi sắp xếp theo điểm cuối bên phải giảm dần trong phạm vi điểm cuối bên trái bằng nhau, đoạn (1,10) xuất hiện đầu tiên. Khi quá trình quét tiến triển,`best_r`trở thành 10 ngay lập tức và tất cả những số khác được phát hiện chính xác như được chứa. Nếu không có loại thứ cấp, phân đoạn ngắn hơn có thể trở thành phân đoạn "tốt nhất" và ngăn cản việc phát hiện. 

Một trường hợp khác liên quan đến việc tăng khoảng thời gian một cách nghiêm ngặt:```
3
1 2
2 3
3 4
```Đây,`best_r`luôn tăng và không có phân khúc nào thỏa mãn`r <= best_r`vào một thời điểm có ý nghĩa. Thuật toán đi đến cuối cùng một cách chính xác và trả về`-1 -1`, phản ánh sự vắng mặt của sự ngăn chặn.
