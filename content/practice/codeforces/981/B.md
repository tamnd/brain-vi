---
problem: 981B
contest_id: 981
problem_index: B
name: "Businessmen Problems"
contest_name: "Avito Code Challenge 2018"
rating: 1000
tags: ["sortings"]
answer: passed_samples
verified: true
solve_time_s: 72
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a328490-05a4-83ec-983e-9f0d55973968
---

#CF 981B - Vấn Đề Của Doanh Nhân 

**Đánh giá:** 1000 
**Thẻ:** sắp xếp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a328490-05a4-83ec-983e-9f0d55973968 

--- 

## Giải pháp 

## Hiểu vấn đề 

Hai công ty đều sở hữu một bộ sưu tập các nguyên tố hóa học, trong đó mỗi nguyên tố được xác định bằng một khóa số nguyên lớn. Mỗi lần xuất hiện của một yếu tố cũng mang lại giá trị lợi nhuận mà công ty kiếm được nếu quyết định trưng bày yếu tố đó tại triển lãm. Các công ty muốn chọn các tập hợp con của các phần tử của riêng họ để tối đa hóa tổng lợi nhuận, nhưng họ bị cấm trình bày cùng một khóa phần tử trong cả hai tập hợp con đã chọn. 

Từ quan điểm mang tính cấu trúc hơn, mọi chìa khóa thành phần có thể xuất hiện ở một công ty, công ty kia hoặc cả hai. Nếu nó xuất hiện ở cả hai, xung đột buộc chúng ta phải quyết định nhiều nhất một bên được phép sử dụng nó. Nếu nó chỉ xuất hiện ở một công ty thì công ty đó có thể tự do đưa vào hoặc loại trừ tùy theo việc nó có lợi hay không. 

Mục tiêu là giải quyết tất cả xung đột giữa các khóa chung theo cách tối đa hóa tổng lợi nhuận đã chọn. 

Các ràng buộc lên tới tổng số 200.000 phần tử, loại trừ mọi giải pháp so sánh mọi phần tử của tập hợp này với mọi phần tử của tập hợp kia. Phương pháp ghép nối bậc hai sẽ yêu cầu thứ tự 10¹⁰ phép toán trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Gợi ý cấu trúc chính là sự tương tác giữa hai bộ chỉ xảy ra thông qua sự bằng nhau của các khóa, vì vậy tất cả lý do có thể được bản địa hóa cho mỗi chỉ mục phần tử duy nhất. 

Một ý tưởng tham lam ngây thơ luôn chọn giá trị cao hơn trong số hai giá trị xung đột cho mỗi khóa có vẻ đầy hứa hẹn nhưng có thể bị áp dụng sai nếu được triển khai mà không nhóm theo khóa trước. Ví dụ: nếu các bản sao của cùng một khóa được xử lý độc lập thay vì tổng hợp, người ta có thể chọn sai nhiều đóng góp cho cùng một danh tính phần tử, điều này vi phạm điều kiện của vấn đề. 

Một trường hợp thất bại tinh vi khác xảy ra khi một chìa khóa chỉ xuất hiện ở một công ty. Việc triển khai bất cẩn chỉ xử lý các khóa giao nhau có thể quên bao gồm toàn bộ các phần tử duy nhất đó, làm mất đi lợi nhuận được đảm bảo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xem xét từng khóa thành phần một cách độc lập và quyết định mọi nhiệm vụ có thể có cho dù nó thuộc về ChemForces, TopChemist hay không. Nếu chúng ta tưởng tượng k khóa dùng chung, điều này sẽ trở thành quyết định ba lần cho mỗi khóa, dẫn đến 3ᵏ khả năng. Ngay cả ở mức k vừa phải, điều này vẫn bùng nổ theo kiểu tổ hợp và không khả thi. 

Quan sát quan trọng là sự kết hợp duy nhất giữa các quyết định là dựa trên chỉ số phần tử giống hệt nhau. Khi chúng tôi nhóm tất cả các lần xuất hiện theo khóa, mỗi khóa sẽ trở thành một đơn vị quyết định độc lập. Đối với một khóa nhất định, chúng tôi lấy đóng góp của ChemForces hoặc đóng góp của TopChemist hoặc tối đa cả hai nếu cả hai đều tồn tại. 

Điều này biến toàn bộ vấn đề thành một nhiệm vụ tổng hợp đơn giản: với mỗi khóa riêng biệt, hãy tính toán mức đóng góp tốt nhất có thể đạt được và tính tổng nó. 

Brute-force không thành công vì nó xử lý các tương tác trên toàn cầu, trong khi vấn đề thực sự được phân tích hoàn hảo thành các lựa chọn độc lập cho mỗi phím. Giải pháp tối ưu khai thác trực tiếp sự phân rã này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^k) | O(k) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phần tử từ cả hai công ty và nhóm chúng theo giá trị chỉ mục của chúng. Đối với mỗi khóa, hãy lưu trữ lợi nhuận tốt nhất được thấy từ ChemForces và lợi nhuận tốt nhất được thấy từ TopChemist. Điều này là cần thiết vì mỗi công ty đảm bảo các khóa riêng biệt trong nội bộ, do đó, chỉ tồn tại tối đa một giá trị cho mỗi khóa của mỗi công ty. 
2. Đối với mỗi khóa trong tổ hợp khóa của cả hai công ty, hãy xác định đóng góp của nó cho câu trả lời cuối cùng. Nếu một khóa chỉ tồn tại ở một công ty, hãy trực tiếp lấy giá trị của nó. Nếu nó tồn tại ở cả hai, lấy giá trị lớn hơn trong hai giá trị. 
3. Tính tổng những đóng góp đã chọn này trên tất cả các khóa.

Quyết định quan trọng là mức tối đa cho mỗi khóa, đảm bảo rằng không có khóa xung đột nào được tính hai lần trong khi vẫn duy trì lựa chọn tốt nhất có thể. 

### Tại sao nó hoạt động 

Mỗi khóa độc lập với tất cả các khóa khác vì hạn chế duy nhất là không thể chọn cùng một khóa từ cả hai công ty. Không có hạn chế về phím chéo. Khi chúng tôi cô lập một khóa, quyết định tối ưu cho khóa đó sẽ không ảnh hưởng đến bất kỳ khóa nào khác, điều này đảm bảo rằng các lựa chọn tối ưu cục bộ sẽ kết hợp thành một giải pháp tối ưu toàn cầu. Đây là thuộc tính có thể phân tách cổ điển: hàm mục tiêu có tính cộng đối với các khóa và các ràng buộc không ghép các khóa khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
best = {}

for _ in range(n):
    a, x = map(int, input().split())
    best[a] = x  # ChemForces values

m = int(input())
for _ in range(m):
    b, y = map(int, input().split())
    if b in best:
        best[b] = max(best[b], y)
    else:
        best[b] = y

print(sum(best.values()))
```Vòng lặp đầu tiên khởi tạo một từ điển có các giá trị của ChemForces. Vòng lặp thứ hai chèn khóa mới cho các phần tử chỉ dành cho TopChemist hoặc cập nhật các phần tử hiện có bằng cách lấy giá trị tối đa để giải quyết xung đột một cách chính xác. 

Tổng cuối cùng tổng hợp mức đóng góp tối ưu cho mỗi khóa. 

Một cạm bẫy triển khai phổ biến là quên rằng đầu vào của mỗi công ty đã đảm bảo tính duy nhất của các khóa bên trong chính nó. Điều đó cho phép chúng tôi lưu trữ một giá trị duy nhất cho mỗi khóa của mỗi công ty mà không cần tổng hợp thêm. Một điểm tinh tế khác là đảm bảo rằng chúng ta không bao giờ đếm gấp đôi một khóa dùng chung bằng cách cộng cả hai giá trị; thay vào đó chúng tôi ghi đè hoặc so sánh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
7 2
3 10
4
1 4
2 4
3 4
4 4
```Chúng tôi theo dõi giá trị trên mỗi khóa: 

| Chìa khóa | Chem | Đầu trang | Được chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | 4 | 
| 7 | 2 | - | 2 | 
| 3 | 10 | 4 | 10 | 
| 2 | - | 4 | 4 | 
| 4 | - | 4 | 4 | 

Tổng cuối cùng là 4 + 2 + 10 + 4 + 4 = 24. 

Điều này xác nhận rằng các khóa chia sẻ chọn chính xác mức đóng góp tối đa trong khi các khóa duy nhất luôn được bao gồm. 

### Ví dụ 2 

đầu vào:```
2
5 100
10 1
3
5 50
7 20
10 10
```| Chìa khóa | Hóa chất | Đầu trang | Được chọn | 
| --- | --- | --- | --- | 
| 5 | 100 | 50 | 100 | 
| 10 | 1 | 10 | 10 | 
| 7 | - | 20 | 20 | 

Câu trả lời cuối cùng là 100 + 10 + 20 = 130. 

Điều này cho thấy cách thuật toán xử lý sự chồng chéo bất đối xứng trong đó một bên chiếm ưu thế hoặc chỉ tồn tại một bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi phần tử được xử lý một lần và các phép toán từ điển ở mức trung bình O(1) | 
| Không gian | O(n + m) | Từ điển lưu trữ một mục nhập cho mỗi khóa phần tử riêng biệt | 

Quá trình quét tuyến tính khớp trực tiếp với các giới hạn kích thước đầu vào, điều này đảm bảo giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    best = {}

    for _ in range(n):
        a, x = map(int, input().split())
        best[a] = x

    m = int(input())
    for _ in range(m):
        b, y = map(int, input().split())
        if b in best:
            best[b] = max(best[b], y)
        else:
            best[b] = y

    return str(sum(best.values()))

# provided sample
assert run("""3
1 2
7 2
3 10
4
1 4
2 4
3 4
4 4
""") == "24"

# single overlap
assert run("""1
1 5
1
1 10
""") == "10"

# disjoint sets
assert run("""2
1 1
2 2
2
3 3
4 4
""") == "10"

# all overlap, alternating dominance
assert run("""3
1 5
2 7
3 1
3
1 6
2 3
3 10
""") == "21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo đơn | 10 | lựa chọn tối đa chính xác trên một phím | 
| bộ rời rạc | 10 | bao gồm tất cả các yếu tố độc đáo | 
| sự thống trị xen kẽ | 21 | chính xác tối đa mỗi phím trên nhiều lớp phủ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một công ty không có thành phần nào. Trong trường hợp đó, câu trả lời chỉ đơn giản là tổng giá trị của công ty kia. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các khóa chỉ đến từ một phía và được thêm trực tiếp. 

Một trường hợp khác là khi tất cả các phím trùng nhau. Đối với đầu vào:```
2
1 5
2 3
2
1 4
2 10
```từ điển phát triển khi khóa 1 trở thành max(5,4)=5 và khóa 2 trở thành max(3,10)=10, mang lại 15. Điều này xác nhận rằng các bản cập nhật không bao giờ tích lũy cả hai giá trị. 

Trường hợp cạnh cuối cùng là các giá trị rất lớn của n và m lên tới 100.000 mỗi trường hợp. Việc tổng hợp dựa trên từ điển đảm bảo xử lý thời gian liên tục cho mỗi phần tử, do đó, ngay cả các đầu vào chồng chéo hoàn toàn vẫn duy trì thời gian tuyến tính và không bị suy giảm.
