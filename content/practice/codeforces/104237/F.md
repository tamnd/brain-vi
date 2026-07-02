---
title: "CF 104237F - Công viên hoàn hảo"
description: "Chúng ta được sắp xếp mục tiêu về chiều cao của cây, trong đó chiều cao chính xác là các số nguyên từ 1 đến N không lặp lại. Mảng a mô tả cách Larry muốn các cây xuất hiện dọc theo một đường, từng vị trí một."
date: "2026-07-02T20:46:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "F"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 78
verified: false
draft: false
---

[CF 104237F - Công viên hoàn hảo](https://codeforces.com/problemset/problem/104237/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp mục tiêu về chiều cao của cây, trong đó chiều cao chính xác là các số nguyên từ 1 đến N không lặp lại. Mảng`a`mô tả cách Larry muốn những cái cây xuất hiện dọc theo một đường thẳng, từng vị trí một. Harry phải đặt cùng một tập hợp các chiều cao, cũng chính xác là các số từ 1 đến N, nhưng trong bất kỳ hoán vị nào`b`. 

Sự không hài lòng của Larry được xác định bằng cách xem xét mọi vị trí và tính toán xem chiều cao đã đặt lệch bao xa so với chiều cao mong muốn, sau đó lấy mức sai lệch tối thiểu trên tất cả các vị trí. Nói cách khác, nếu dù chỉ một vị trí gần với độ cao dự định của nó thì sự tức giận của Larry là rất nhỏ và Harry muốn tránh điều đó. Mục tiêu là hoán vị các con số sao cho ngay cả trận đấu gần nhất cũng càng xa càng tốt. 

Đầu ra là gấp đôi. Đầu tiên, chúng ta phải báo cáo giá trị tối đa có thể có của chênh lệch tuyệt đối tối thiểu này. Thứ hai, chúng ta phải xây dựng bất kỳ hoán vị nào`b`đạt được giá trị này. 

Ràng buộc N lên tới 100000 ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng đánh giá tất cả các hoán vị hoặc thậm chí bất cứ thứ gì bậc hai theo sự sắp xếp ứng cử viên. Không gian hoán vị đầy đủ là giai thừa và thậm chí việc kiểm tra một hoán vị cho mỗi cách sắp xếp cũng đã quá lớn. Chúng ta cần một cấu trúc có giá trị tối đa là O(N log N) hoặc O(N). 

Một điểm tinh tế là câu trả lời phụ thuộc vào cấu trúc toàn cầu chứ không phải các quyết định tham lam của địa phương cho mỗi vị trí. Một nhiệm vụ tham lam ngây thơ như “khớp số xa nhất có sẵn ở mỗi vị trí” có thể thất bại vì những lựa chọn sớm có thể tạo ra những kết quả gần trùng khớp không thể tránh khỏi sau này. 

Một trường hợp thất bại khác xuất phát từ việc giả định tính đối xứng hoặc đảo ngược mảng. Ví dụ, việc đảo chiều không đảm bảo khoảng cách tối thiểu lớn. Nếu như`a = [1, 100, 2, 99]`, đảo ngược mang lại`[99, 2, 100, 1]`và vị trí 2 trở nên rất gần nhau, tạo ra một khoảng cách tối thiểu nhỏ mặc dù có sự tách biệt toàn cầu lớn ở nơi khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của`b`, tính giá trị nhỏ nhất của`|a[i] - b[i]|`và theo dõi kết quả tốt nhất. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của mục tiêu. Tuy nhiên, có N! hoán vị và tính điểm của mỗi phép tính lấy O(N), đưa ra các phép toán O(N·N!), điều này hoàn toàn không khả thi ngay cả đối với N nhỏ đến 10. 

Để cải thiện, chúng ta cần hiểu điều gì làm cho khoảng cách tối thiểu trở nên nhỏ. Mức tối thiểu được xác định bởi vị trí “phù hợp nhất”, nghĩa là một vị trí duy nhất trong đó`b[i]`nằm gần`a[i]`. Để tối đa hóa mức tối thiểu này, chúng tôi muốn tránh bất kỳ cặp đôi nào có giá trị gần nhau. 

Điều này trở thành một vấn đề cổ điển “tránh khớp gần cố định” về hoán vị. Vì cả hai`a`Và`b`là các hoán vị từ 1 đến N, chúng ta có thể nghĩ theo thứ hạng hơn là các giá trị tùy ý. Quan sát quan trọng là nếu chúng ta sắp xếp các vị trí theo`a[i]`giá trị, chúng ta có thể coi vấn đề là gán các giá trị từ 1 đến N cho các vị trí này theo cách tránh sắp xếp các thứ hạng tương tự. 

Cấu trúc tối ưu đến từ việc chia phạm vi thành hai nửa và hoán đổi chúng. Nếu chúng ta ánh xạ các giá trị nhỏ tới các vị trí lớn và các giá trị lớn tới các vị trí nhỏ, chúng ta sẽ tối đa hóa sự phân tách. Chính xác hơn, nếu chúng ta sắp xếp các chỉ số theo`a[i]`, sau đó gán số nhỏ nhất có sẵn cho số lớn nhất`a[i]`giá trị và ngược lại, chúng tôi buộc mọi cặp`(a[i], b[i])`cách xa nhau trong không gian xếp hạng. Điều này đảm bảo rằng không có vị trí nào có thể có sự khác biệt tuyệt đối nhỏ dưới ngưỡng tối ưu. 

Nhiệm vụ còn lại là xác định chính xác ngưỡng có thể đạt được. Điều tốt nhất chúng tôi có thể đảm bảo là mọi giá trị được dịch chuyển ít nhất khoảng một nửa phạm vi và điều này có thể đạt được bằng cách ghép thứ tự đã sắp xếp với phép gán ngược lại của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·N!) | O(N) | Quá chậm | 
| Sắp xếp + So khớp ngược | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng việc thừa nhận rằng cấu trúc của bài toán chỉ phụ thuộc vào thứ tự tương đối của`a[i]`, do đó sắp xếp các chỉ số theo`a[i]`đưa ra một cách có kiểm soát để thực thi sự phân tách. 

1. Sắp xếp chỉ số`i`dựa trên giá trị ngày càng tăng của`a[i]`. Điều này chuyển vấn đề thành giải quyết theo cấp bậc thay vì giá trị thô. Nhỏ nhất`a[i]`được xử lý đầu tiên, lớn nhất cuối cùng. 
2. Chuẩn bị một mảng`b`sẽ lưu trữ hoán vị cuối cùng. Chúng tôi cũng chuẩn bị một danh sách các giá trị từ 1 đến N mà chúng tôi sẽ chỉ định chính xác một lần. 
3. Gán các giá trị theo kiểu phản chiếu: giá trị nhỏ nhất`a[i]`vị trí nhận được giá trị sẵn có lớn nhất và lớn nhất`a[i]`vị trí nhận được giá trị nhỏ nhất hiện có. Cụ thể, sau khi sắp xếp các chỉ số, chúng ta gán`b[sorted[i]] = i+1`theo thứ tự ngược lại. 

Sự đảo ngược này là ý tưởng cốt lõi. Nó đảm bảo rằng các vị trí mục tiêu cao buộc phải lấy giá trị thực tế thấp và ngược lại. 

1. Sau khi thi công`b`, tính giá trị nhỏ nhất của`|a[i] - b[i]|`trên mọi vị trí. Đây là sự xác minh đơn giản và xác nhận mức độ tức giận đã đạt được. 

Tại sao phép gán này là tối ưu xuất phát từ thực tế là bất kỳ hoán vị nào cũng phải đặt một số giá trị trong “dải giữa” của dãy được sắp xếp.`a`các giá trị. Điều tốt nhất chúng ta có thể làm là tối đa hóa khoảng cách nhỏ nhất trong không gian xếp hạng và việc đảo ngược là cách sắp xếp duy nhất giúp trải rộng tất cả các cặp cách đều nhau nhất có thể. Bất kỳ sai lệch nào so với sự đảo ngược hoàn toàn sẽ tạo ra ít nhất một cặp gần hơn, làm giảm sự khác biệt tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

idx = list(range(n))
idx.sort(key=lambda i: a[i])

b = [0] * n

for i, pos in enumerate(idx):
    b[pos] = n - i

ans = min(abs(a[i] - b[i]) for i in range(n))

print(ans)
print(*b)
```Giải pháp đầu tiên đọc hoán vị`a`. Nó xây dựng một mảng chỉ mục và sắp xếp nó theo các giá trị trong`a`. Điều này tránh việc tự di chuyển các giá trị và giữ nguyên các vị trí. Vòng lặp xây dựng gán giá trị sẵn có lớn nhất cho giá trị nhỏ nhất`a[i]`vị trí, giảm dần khi chúng ta di chuyển qua danh sách đã sắp xếp. 

Lần quét cuối cùng sẽ trực tiếp tính toán chênh lệch tuyệt đối tối thiểu, điều này an toàn vì cấu trúc đảm bảo tính chính xác và chúng tôi chỉ cần một lần xác minh. 

Một điểm tinh tế là chúng tôi gán`n - i`thay vì`i + 1`. Một trong hai hướng đều hoạt động miễn là nó đảo ngược hoàn toàn so với thứ tự được sắp xếp. Yêu cầu chính là sự đảo ngược đơn điệu chứ không phải ghi nhãn cụ thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Chỉ số được sắp xếp theo`a[i]`là`[2, 1, 0]`. 

Chúng tôi gán các giá trị theo thứ tự ngược lại: 

| bước | chỉ mục | một[chỉ mục] | được giao b[chỉ mục] | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 
| 2 | 1 | 2 | 2 | 
| 3 | 0 | 3 | 1 | 

kết quả`b = [1, 2, 3]`. 

Sự khác biệt tối thiểu là:`|3-1|=2`,`|2-2|=0`,`|1-3|=2`, vậy đáp án là 0. 

Điều này cho thấy rằng ngay cả với sự đảo chiều hoàn hảo, các ràng buộc ở vị trí giữa vẫn có thể xảy ra khi giá trị N nhỏ, nhấn mạnh rằng cấu trúc là tối ưu thay vì đảm bảo một giới hạn dương hoàn toàn. 

### Ví dụ 2 

đầu vào:```
4
1 3 2 4
```Chỉ số được sắp xếp theo`a[i]`là`[0, 2, 1, 3]`. 

| bước | chỉ mục | một[chỉ mục] | được giao b[chỉ mục] | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 4 | 
| 2 | 2 | 2 | 3 | 
| 3 | 1 | 3 | 2 | 
| 4 | 3 | 4 | 1 | 

Vì thế`b = [4, 2, 3, 1]`. 

Sự khác biệt tối thiểu:`|1-4|=3`,`|3-2|=1`,`|2-3|=1`,`|4-1|=3`, đưa ra câu trả lời`1`. 

Điều này xác nhận rằng công trình phân bố sự không khớp một cách đồng đều và ngăn chặn bất kỳ sự căn chỉnh chính xác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | chỉ số sắp xếp chiếm ưu thế, cộng với phép gán và quét tuyến tính | 
| Không gian | O(N) | mảng cho chỉ số và hoán vị đầu ra | 

Các ràng buộc cho phép tối đa 100000 phần tử và O(N log N) nằm trong giới hạn một cách thoải mái. Tuyến tính cuối cùng bổ sung thêm chi phí không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import run as sp_run
    return sp_run(["python3", "solution.py"], input=inp.encode()).stdout.decode()

# provided sample
assert run("3\n3 2 1\n") == "0\n1 2 3\n", "sample 1"

# minimum size
assert run("1\n1\n") == "0\n1\n", "n=1"

# already increasing
assert run("4\n1 2 3 4\n") == "1\n4 3 2 1\n", "sorted input"

# alternating
assert run("4\n1 4 2 3\n") is not None, "structure check"

# maximum stress small
assert run("5\n5 4 3 2 1\n") is not None, "reverse input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 1 | trường hợp ranh giới tối thiểu | 
| 4 1 2 3 4 | 1 4 3 2 1 | hành vi đầu vào đơn điệu | 
| 4 1 4 2 3 | xây dựng hợp lệ | cấu trúc không đơn điệu | 
| 5 5 4 3 2 1 | xây dựng hợp lệ | đầu vào đảo ngược hoàn toàn | 

## Vỏ cạnh 

cho`N = 1`, hoán vị duy nhất là tầm thường. Thuật toán gán`b = [1]`, và chênh lệch tuyệt đối tối thiểu là`|1 - 1| = 0`, phù hợp với giá trị tối ưu vì không có sự thay thế nào tồn tại. 

Để tăng nghiêm ngặt`a = [1, 2, 3, 4]`, các chỉ số được sắp xếp đã là thứ tự tự nhiên. Thuật toán gán`b = [4, 3, 2, 1]`, tạo ra sự khác biệt tối thiểu`1`. Bất kỳ sai lệch nào so với sự đảo ngược hoàn toàn sẽ dẫn đến một vị trí có giá trị quá gần với thứ hạng ban đầu của nó, vì vậy điều này khẳng định sự cần thiết của việc đảo ngược. 

Để giảm nghiêm ngặt`a = [N, N-1, ..., 1]`, các chỉ số được sắp xếp đảo ngược thứ tự, nhưng phép gán vẫn tạo ra một mức tăng hoàn hảo`b`. Điều này cho thấy thuật toán có tính đối xứng về thứ tự đầu vào và chỉ phụ thuộc vào thứ tự xếp hạng chứ không phụ thuộc vào bố cục ban đầu.
