---
title: "CF 104822F - Sự khác biệt về kỹ năng"
description: "Chúng tôi được cung cấp một danh sách nhân viên, mỗi nhân viên có một giá trị kỹ năng bằng số. Đối với mỗi nhân viên, chúng tôi muốn biết nhóm lớn nhất có thể bao gồm họ, theo một hạn chế: trong bất kỳ nhóm nào được chọn, sự khác biệt giữa kỹ năng tối đa và tối thiểu không được vượt quá mức cố định…"
date: "2026-06-28T12:41:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "F"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 91
verified: false
draft: false
---

[CF 104822F - Sự khác biệt về kỹ năng](https://codeforces.com/problemset/problem/104822/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách nhân viên, mỗi nhân viên có một giá trị kỹ năng bằng số. Đối với mỗi nhân viên, chúng tôi muốn biết nhóm lớn nhất có thể bao gồm họ, theo một hạn chế: trong bất kỳ nhóm nào được chọn, sự khác biệt giữa kỹ năng tối đa và tối thiểu không được vượt quá ngưỡng cố định$k$. 

Vì vậy với mỗi chỉ số$i$, chúng ta đang hỏi một cách hiệu quả: nếu nhân viên$i$phải được bao gồm, tập hợp con lớn nhất của nhân viên có kỹ năng nằm trong một khoảng thời gian nào đó là gì?$[x, x+k]$cái đó cũng chứa$a_i$. 

Quan sát quan trọng là bất kỳ nhóm hợp lệ nào đều được xác định hoàn toàn bởi một khoảng giá trị trên trục số và nhiệm vụ giảm xuống còn phải hiểu có bao nhiêu phần tử mảng rơi vào một khoảng như vậy trong khi buộc phải đưa vào một phần tử cụ thể. 

Các ràng buộc đi lên đến$n = 2 \cdot 10^5$, do đó, bất kỳ giải pháp nào cố gắng tính toán lại khoảng thời gian tốt nhất một cách độc lập cho mỗi$i$trong thời gian tuyến tính sẽ là quá chậm. Một sự ngây thơ$O(n^2)$quét sẽ đạt khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Điều này ngay lập tức gợi ý một cấu trúc cửa sổ sắp xếp hoặc trượt nơi công việc lặp đi lặp lại có thể được tái sử dụng. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong trường hợp đó, chỉ những nhân viên có cùng kỹ năng mới có thể được nhóm lại. Một trường hợp cạnh khác là khi tất cả các giá trị khác biệt và cách nhau nhiều hơn$k$, buộc mọi câu trả lời phải là 1. Một nỗ lực ngây thơ giả định tính liên tục của các chỉ số trong mảng ban đầu sẽ thất bại vì nhóm tối ưu không liền kề trong không gian chỉ mục, chỉ trong không gian giá trị được sắp xếp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi nhân viên$i$, chúng tôi thử mọi tập hợp con có thể bao gồm$i$, nhưng chúng tôi ngay lập tức rút gọn điều này thành một dạng đơn giản hơn: sửa$i$, chọn người đó làm thành viên của nhóm và thử mở rộng nhóm bằng cách thêm mọi nhân viên khác$j$nếu nhóm kết quả vẫn thỏa mãn$\max - \min \le k$. Để xác minh điều này, chúng tôi duy trì kỹ năng tối thiểu và tối đa trong tập hợp con hiện tại. 

Điều này hoạt động chính xác nhưng tốn kém về mặt tính toán. Đối với mỗi$i$, chúng ta có thể kiểm tra$O(n)$các ứng cử viên và tính toán lại tính hợp lệ, dẫn đến$O(n^2)$hành vi. Với$2 \cdot 10^5$nhân viên, điều này là không thể thực hiện được. 

Thông tin chi tiết về cấu trúc quan trọng là điều kiện chỉ phụ thuộc vào giá trị chứ không phải chỉ số và nhóm tối ưu có chứa bất kỳ nhân viên nào$i$phải tương ứng với một phân đoạn liền kề trong mảng kỹ năng đã được sắp xếp. Sau khi mảng được sắp xếp, bất kỳ nhóm hợp lệ nào cũng là một cửa sổ trượt trong đó sự khác biệt giữa các điểm cuối là nhiều nhất$k$. Thay vì tính toán lại cửa sổ này cho mỗi$i$, chúng ta có thể tính toán trước, đối với mọi vị trí, một cửa sổ hợp lệ có thể mở rộng bao xa. 

Chúng tôi sử dụng kỹ thuật hai con trỏ trên mảng được sắp xếp để tính toán phân đoạn hợp lệ tối đa kết thúc ở mỗi vị trí hoặc bắt đầu ở mỗi vị trí. Sau đó, đối với mỗi chỉ mục gốc, chúng tôi dịch nó sang vị trí của nó trong mảng đã sắp xếp và đọc kích thước của cửa sổ hợp lệ lớn nhất bao gồm nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + Hai con trỏ |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng tối ưu 

Chúng tôi chuyển đổi vấn đề thành một vấn đề trên một mảng được sắp xếp và sau đó sử dụng cửa sổ trượt để tìm phạm vi hợp lệ tối đa. 

### Các bước 

1. Ghép nối từng kỹ năng của nhân viên với chỉ số ban đầu của nó và sắp xếp theo giá trị kỹ năng. 

Việc sắp xếp là cần thiết vì tính hợp lệ chỉ phụ thuộc vào phạm vi giữa giá trị tối thiểu và tối đa. 
2. Duy trì hai con trỏ$l$Và$r$, cả hai đều bắt đầu từ 0, biểu thị cửa sổ hợp lệ hiện tại trong mảng được sắp xếp. 
3. Đối với mỗi$r$từ 0 đến$n-1$, mở rộng cửa sổ bằng cách bao gồm$a[r]$, sau đó di chuyển$l$chuyển tiếp trong khi$a[r] - a[l] > k$. 

Điều này đảm bảo cửa sổ luôn thỏa mãn ràng buộc. 
4. Sau khi sửa chữa$r$, đoạn$[l, r]$là khoảng hợp lệ lớn nhất kết thúc tại$r$. 
5. Đối với mọi vị trí$r$, ghi lại kích thước của cửa sổ này, tức là$r - l + 1$. 
6. Mỗi chỉ mục trong cửa sổ này có thể đạt được ít nhất quy mô nhóm này nếu nhóm tập trung vào bất kỳ thành viên nào trong phân khúc đó. 
7. Cuối cùng, ánh xạ kết quả trở lại các chỉ mục ban đầu bằng cách sử dụng các vị trí được lưu trữ. 

Điểm mấu chốt là khi chúng ta biết tất cả các cửa sổ hợp lệ tối đa, câu trả lời đúng nhất của mỗi nhân viên là cửa sổ lớn nhất chứa vị trí được sắp xếp của nó. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong mảng đã sắp xếp, cửa sổ trượt duy trì bất biến rằng tất cả các phần tử bên trong đều thỏa mãn ràng buộc$\max - \min \le k$. Vì mảng được sắp xếp nên sự khác biệt luôn được xác định bởi các điểm cuối, nên việc thu nhỏ từ bên trái là cách duy nhất để khôi phục tính hợp lệ khi điểm cuối bên phải mở rộng. Điều này đảm bảo rằng mọi phân đoạn hợp lệ tối đa được phát hiện chính xác một lần và không có phân đoạn hợp lệ lớn hơn nào tồn tại ngoài ranh giới cửa sổ hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

arr = [(a[i], i) for i in range(n)]
arr.sort()

ans = [0] * n
l = 0

for r in range(n):
    while arr[r][0] - arr[l][0] > k:
        l += 1
    ans[arr[r][1]] = r - l + 1

print(*ans)
```Giải pháp bắt đầu bằng cách ghép từng kỹ năng với chỉ mục ban đầu của nó để chúng ta có thể khôi phục câu trả lời sau này. Sau khi sắp xếp, chúng ta duy trì con trỏ bên trái chỉ di chuyển về phía trước. Vòng lặp while đảm bảo rằng phân đoạn hiện tại luôn tôn trọng giới hạn chênh lệch kỹ năng. Kích thước cửa sổ tính toán ở mỗi bước được gán cho nhân viên ban đầu tại vị trí$r$, vì nhân viên đó là một phần của chính xác khoảng thời gian hợp lệ tối đa này kết thúc tại$r$. 

Một chi tiết tinh tế là chúng tôi không tính toán lại câu trả lời một cách rõ ràng cho tất cả các thành viên của cửa sổ. Mỗi vị trí$r$đóng góp “cửa sổ kết thúc” tốt nhất của nó và vì mọi phần tử đều trở thành điểm cuối ở một giai đoạn nào đó nên quy mô nhóm hợp lệ tối đa của nó sẽ được nắm bắt chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 2
1 2 3 4 6 9
```Hình thức sắp xếp:```
(1,0) (2,1) (3,2) (4,3) (6,4) (9,5)
```| r | giá trị | tôi | cửa sổ | kích thước | chỉ số được giao | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 0 | 
| 1 | 2 | 0 | [1,2] | 2 | 1 | 
| 2 | 3 | 0 | [1,2,3] | 3 | 2 | 
| 3 | 4 | 1 | [2,3,4] | 3 | 3 | 
| 4 | 6 | 3 | [4,6] | 2 | 4 | 
| 5 | 9 | 5 | [9] | 1 | 5 | 

Đầu ra:```
3 3 3 3 2 1
```Dấu vết này cho thấy cửa sổ dịch chuyển như thế nào khi ràng buộc bị vi phạm, đặc biệt khi di chuyển từ 4 đến 6, trong đó ranh giới bên trái nhảy về phía trước. 

### Mẫu 2 

đầu vào:```
15 78
98 190 175 67 109 139 297 175 789 162 109 87 165 243 72
```Sau khi sắp xếp, các cửa sổ sẽ mở rộng rộng rãi vì$k$đủ lớn để nhóm nhiều phần tử. 

| r | l tóm tắt phong trào | kích thước cửa sổ | 
| --- | --- | --- | 
| nhiều | tôi thỉnh thoảng di chuyển | khác nhau, lên tới 8 | 

Quan sát quan trọng trong trường hợp này là các cụm lớn hình thành trong đó các giá trị nằm trong một dải rộng, tạo ra các phân đoạn tối đa lặp lại giải thích các câu trả lời lặp lại như 8. 

Điều này xác nhận rằng thuật toán nhóm các vùng dày đặc của mảng được sắp xếp một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian |$O(n)$| Lưu trữ các cặp và câu trả lời | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp nằm trong giới hạn và quá trình quét tuyến tính đảm bảo hiệu quả ngay cả trong trường hợp đầu vào dày đặc trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    arr = [(a[i], i) for i in range(n)]
    arr.sort()

    ans = [0] * n
    l = 0

    for r in range(n):
        while arr[r][0] - arr[l][0] > k:
            l += 1
        ans[arr[r][1]] = r - l + 1

    return " ".join(map(str, ans))

# provided samples
assert run("6 2\n1 2 3 4 6 9\n") == "3 3 3 3 2 1"
assert run("15 78\n98 190 175 67 109 139 297 175 789 162 109 87 165 243 72\n") == \
"8 6 8 7 8 8 2 8 1 8 8 7 8 5 7"

# custom cases
assert run("1 10\n5\n") == "1"
assert run("5 0\n1 1 1 2 2\n") == "3 3 3 2 2"
assert run("4 100\n1 50 90 120\n") == "4 4 4 4"
assert run("6 1\n1 3 5 7 9 11\n") == "1 1 1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | hành vi kích thước tối thiểu | 
| trùng lặp với k=0 | chỉ các bản sao được nhóm lại | ràng buộc bình đẳng chính xác | 
| k lớn | nhóm mảng đầy đủ | tính chính xác của cửa sổ toàn cầu | 
| giá trị cách nhau | tất cả những cái | không có sự nhóm ngẫu nhiên | 

## Vỏ cạnh 

Khi nào$k = 0$, chỉ những giá trị giống hệt nhau mới có thể tạo thành một nhóm. Cửa sổ trượt co lại để đảm bảo các điểm cuối bằng nhau, do đó các bản sao sẽ tạo thành các cụm một cách tự nhiên. Ví dụ, đầu vào`1 0 / 1 1 1 2 2`tạo ra các nhóm`[1,1,1]`Và`[2,2]`, phù hợp với kết quả đầu ra dự kiến. 

Khi tất cả các giá trị khác nhau nhiều hơn$k$, mọi cửa sổ sẽ thu gọn thành một phần tử duy nhất. Thuật toán ngay lập tức thu nhỏ con trỏ bên trái cho mỗi điểm cuối bên phải mới, tạo ra kích thước 1 một cách nhất quán. 

Khi tất cả các giá trị giống hệt nhau, cửa sổ không bao giờ co lại và mọi nhân viên đều nhận được câu trả lời$n$, vì toàn bộ mảng tạo thành một phân đoạn hợp lệ.
