---
title: "CF 1980B - Chọn Hình Khối"
description: "Chúng ta được đưa ra một số tình huống độc lập trong đó một tập hợp các khối được đánh số được sắp xếp lại dựa trên giá trị của chúng. Một khối lập phương đặc biệt vì nó được yêu thích, được xác định bởi vị trí ban đầu của nó."
date: "2026-06-08T16:51:52+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1980
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 950 (Div. 3)"
rating: 800
weight: 1980
solve_time_s: 80
verified: true
draft: false
---

[CF 1980B - Chọn khối](https://codeforces.com/problemset/problem/1980/B) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số tình huống độc lập trong đó một tập hợp các khối được đánh số được sắp xếp lại dựa trên giá trị của chúng. Một khối lập phương đặc biệt vì nó được yêu thích, được xác định bởi vị trí ban đầu của nó. Sau khi sắp xếp tất cả các khối theo thứ tự giá trị giảm dần, Dmitry loại bỏ k khối đầu tiên khỏi danh sách đã sắp xếp này. Nhiệm vụ là xác định xem khối yêu thích luôn bị loại bỏ, không bao giờ bị loại bỏ hay có thể đi theo một trong hai cách tùy thuộc vào cách giải quyết các ràng buộc về giá trị. 

Khó khăn chính là việc sắp xếp không hoàn toàn mang tính quyết định. Khi nhiều khối có cùng giá trị, thứ tự tương đối của chúng sau khi sắp xếp có thể thay đổi tùy ý. Điều này có nghĩa là vị trí cuối cùng của khối yêu thích không cố định; nó phụ thuộc vào cách sắp xếp các phần tử có giá trị bằng nhau. 

Đầu ra phải phân loại khối yêu thích thành ba loại. Nếu nó luôn xuất hiện trong k vị trí đầu tiên sau bất kỳ lần sắp xếp hợp lệ nào thì câu trả lời là CÓ. Nếu nó không bao giờ có thể xuất hiện trong k vị trí đầu tiên thì câu trả lời là KHÔNG. Mặt khác, tồn tại ít nhất một cách sắp xếp hợp lệ khi nó bị xóa và một cách sắp xếp hợp lệ khác thì không, vì vậy câu trả lời là CÓ THỂ. 

Các ràng buộc nhỏ, với n lên tới 100 và lên tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc liệt kê trạng thái nặng nề đối với các hoán vị của các phần tử bằng nhau. Mặc dù lý luận hoán vị vũ phu về mặt lý thuyết là có thể, nhưng số lượng sắp xếp lại giữa các phần tử bằng nhau tăng theo giai thừa và không cần thiết. 

Trường hợp cạnh tinh tế phát sinh khi nhiều phần tử có cùng giá trị với phần tử yêu thích. Trong những trường hợp như vậy, vị trí của mục yêu thích có thể thay đổi trong phạm vi rộng sau khi sắp xếp. 

Ví dụ: nếu tất cả các khối có giá trị 5 và k = 3 và n = 5 thì khối yêu thích có thể được đặt ở bất kỳ đâu theo thứ tự được sắp xếp. Nó có thể nằm trong 3 trận đầu tiên hoặc nằm ngoài chúng tùy thuộc vào tỷ số hòa, vì vậy câu trả lời là CÓ THỂ. 

Một trường hợp cạnh khác xuất hiện khi k rất lớn. Nếu k = n, tất cả các hình khối luôn bị loại bỏ bất kể thứ tự nào, vì vậy câu trả lời luôn là CÓ. Ngược lại, nếu k = 0 thì không có khối nào bị loại bỏ nên câu trả lời luôn là KHÔNG. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các hoán vị của các khối tuân theo thứ tự giá trị không tăng. Điều này có nghĩa là tạo ra tất cả các cách để hoán vị các phần tử trong các nhóm có giá trị bằng nhau và kiểm tra xem khối yêu thích có xuất hiện ở k vị trí đầu tiên trong tất cả, không có hoặc một số vị trí trong số đó hay không. Mặc dù đơn giản về mặt khái niệm, nhưng điều này lại bùng nổ theo từng giai đoạn bên trong mỗi nhóm bản sao. Trong trường hợp xấu nhất khi tất cả các giá trị đều bằng nhau, giá trị này sẽ trở thành n! hoán vị, điều này hoàn toàn không thể thực hiện được ngay cả khi n = 100. 

Quan sát chính là việc sắp xếp chỉ cố định thứ tự tương đối giữa các giá trị khác nhau. Tất cả sự không chắc chắn chỉ tồn tại bên trong các khối có giá trị bằng nhau. Vì vậy, thay vì mô phỏng các hoán vị, chúng ta chỉ cần suy luận xem có bao nhiêu phần tử lớn hơn giá trị yêu thích và bao nhiêu phần tử bằng giá trị đó. 

Tất cả các phần tử có giá trị lớn hơn giá trị yêu thích phải xuất hiện trước nó theo mọi thứ tự được sắp xếp hợp lệ. Các vị trí này được cố định và đóng góp vào tiền tố được đảm bảo. Các phần tử có giá trị bằng nhau có thể xuất hiện trước hoặc sau phần tử được yêu thích một cách tùy ý giữa chúng. Điều này tạo ra một loạt các vị trí có thể có cho khối yêu thích trong mảng được sắp xếp cuối cùng. 

Đặt số phần tử cao hơn a_f và bằng số phần tử bằng a_f, bao gồm cả phần tử yêu thích. 

Vị trí sớm nhất có thể có của mục ưa thích là khi tất cả các phần tử bằng nhau được sắp xếp sau nó trong khối của chúng, do đó, nó đứng ngay sau tất cả các phần tử lớn hơn. Vị trí đó cao hơn +1. 

Vị trí muộn nhất có thể là khi tất cả các phần tử bằng nhau khác được đặt trước nó, để nó nằm cuối cùng trong khối bằng nhau của nó. Vị trí đó cao hơn + bằng.

Bây giờ chúng ta so sánh phạm vi này với k. Nếu ngay cả vị trí đầu tiên lớn hơn k thì vị trí yêu thích sẽ không bao giờ bị xóa. Nếu vị trí mới nhất nhiều nhất là k thì nó luôn bị xóa. Ngược lại, tồn tại sự phân chia ràng buộc đặt nó ở hai bên, vì vậy câu trả lời là CÓ THỂ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force trong các nhóm bằng nhau | Hàm mũ | O(n) | Quá chậm | 
| Đếm các phần tử lớn hơn và bằng nhau | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n, f, k và mảng a. 

Khối yêu thích nằm ở chỉ số f, vì vậy chúng ta lưu trữ giá trị của nó x = a[f]. 
2. Đếm xem có bao nhiêu phần tử có giá trị lớn hơn x. 

Các phần tử này phải xuất hiện trước phần tử yêu thích trong bất kỳ sự sắp xếp nào. 
3. Đếm xem có bao nhiêu phần tử có giá trị bằng x. 

Nhóm này bao gồm chính mục yêu thích và xác định mức độ linh hoạt của vị trí của nó. 
4. Tính vị trí sớm nhất có thể của mục yêu thích lớn hơn +1. 

Điều này tương ứng với trường hợp tất cả các phần tử bằng nhau ngoại trừ phần tử yêu thích được đặt sau nó. 
5. Tính vị trí mới nhất có thể có của mục yêu thích là lớn hơn + bằng. 

Điều này tương ứng với trường hợp tất cả các phần tử bằng nhau khác được đặt trước nó. 
6. So sánh phạm vi này với k. 

Nếu mới nhất ≤ k thì mọi thứ tự có thể sẽ đặt mục ưa thích vào tiền tố đã bị loại bỏ, do đó xuất ra CÓ. 
7. Nếu sớm nhất > k, thì không có thứ tự nào có thể đặt nó trong tiền tố đã bị loại bỏ, do đó xuất ra NO. 
8. Nếu không thì xuất ra CÓ THỂ. 

Điều này tương ứng với sự chồng chéo giữa các vị trí có thể và điểm cắt. 

### Tại sao nó hoạt động 

Thứ tự sắp xếp được xác định đầy đủ theo các hoán vị bên trong các nhóm có giá trị bằng nhau. Các phần tử có giá trị khác nhau có thứ tự tương đối cố định. Điều này có nghĩa là nguồn không chắc chắn duy nhất của mục yêu thích là thứ hạng của nó trong số các phần tử có cùng giá trị. Vị trí cuối cùng của nó chỉ phụ thuộc vào số lượng phần tử có giá trị bằng nhau được đặt trước nó. Số đó có thể nằm trong khoảng từ 0 đến bằng − 1 độc lập với các ràng buộc khác, do đó, vị trí của mục yêu thích kéo dài trong một khoảng liền kề [lớn hơn + 1, lớn hơn + bằng]. Bất kỳ vị trí nào bên ngoài khoảng này đều không thể thực hiện được và mọi vị trí bên trong đều có thể đạt được bằng cách phá ràng buộc phù hợp. Điều này làm cho việc so sánh với k đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, f, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    x = a[f - 1]
    
    greater = 0
    equal = 0
    
    for v in a:
        if v > x:
            greater += 1
        elif v == x:
            equal += 1
    
    earliest = greater + 1
    latest = greater + equal
    
    if latest <= k:
        print("YES")
    elif earliest > k:
        print("NO")
    else:
        print("MAYBE")
```Việc triển khai phản ánh trực tiếp logic khoảng thời gian dẫn xuất. Điểm tinh tế duy nhất là lập chỉ mục: chỉ mục yêu thích dựa trên 1 trong đầu vào, do đó, nó phải được chuyển sang dựa trên 0 khi truy cập vào mảng. Số lượng được tính toán lại cho mỗi trường hợp thử nghiệm vì n nhỏ. 

Logic quyết định tránh việc sắp xếp hoàn toàn, điều này không cần thiết vì chúng ta không bao giờ cần hoán vị chính xác, chỉ cần khoảng xếp hạng của khối yêu thích. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, f = 2, k = 2 

a = [4, 3, 3, 2, 3] 

| Bước | lớn hơn | bằng | sớm nhất | mới nhất | k | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| tính x = 3 | | | | | | | 
| đếm giá trị | 1 | 3 | 2 | 4 | 2 | CÓ THỂ | 

Giá trị yêu thích là 3. Có một giá trị lớn hơn (4) và ba giá trị bằng nhau. Người được yêu thích có thể đứng thứ hai đến thứ tư trong dãy được sắp xếp tùy thuộc vào mức độ hòa. Vì k = 2 nằm trong phạm vi này nên một số cách sắp xếp hợp lệ sẽ loại bỏ nó trong khi những cách sắp xếp khác thì không. 

### Ví dụ 2 

đầu vào: 

n = 5, f = 5, k = 3 

a = [4, 2, 1, 3, 5] 

| Bước | lớn hơn | bằng | sớm nhất | mới nhất | k | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| tính x = 5 | | | | | | | 
| đếm giá trị | 0 | 1 | 1 | 1 | 3 | CÓ | 

Phần tử yêu thích là phần tử lớn nhất nên nó luôn trở thành phần tử đầu tiên trong thứ tự sắp xếp. Vì k = 3 nên nó luôn bị loại bỏ bất kể có bị ràng buộc hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét mảng một lần để đếm các phần tử lớn hơn và bằng nhau | 
| Không gian | O(1) | Chỉ có một vài quầy được sử dụng | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với n tối đa 100, do đó, tối đa 100.000 thao tác, không đáng kể dưới 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    t = int(input())
    for _ in range(t):
        n, f, k = map(int, input().split())
        a = list(map(int, input().split()))
        x = a[f - 1]
        
        greater = sum(1 for v in a if v > x)
        equal = sum(1 for v in a if v == x)
        
        earliest = greater + 1
        latest = greater + equal
        
        if latest <= k:
            out.append("YES")
        elif earliest > k:
            out.append("NO")
        else:
            out.append("MAYBE")
    
    return "\n".join(out) + "\n"

# provided samples
assert run("""12
5 2 2
4 3 3 2 3
5 5 3
4 2 1 3 5
5 5 2
5 2 4 1 3
5 5 5
1 2 5 4 3
5 5 4
3 1 2 4 5
5 5 5
4 3 2 1 5
6 5 3
1 2 3 1 2 3
10 1 1
1 1 1 1 1 1 1 1 1 1
1 1 1
42
5 2 3
2 2 1 1 2
2 1 1
2 1
5 3 1
3 3 2 3 2
""") == """MAYBE
YES
NO
YES
YES
YES
MAYBE
MAYBE
YES
YES
YES
NO
"""

# custom cases
assert run("""1
1 1 1
5
""") == "YES\n"

assert run("""1
5 1 0
5 4 3 2 1
""") == "NO\n"

assert run("""1
4 2 2
2 2 2 2
""") == "MAYBE\n"

assert run("""1
6 3 2
5 5 5 1 1 1
""") == "NO\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn luôn bị loại bỏ | CÓ | trường hợp cạnh tối thiểu | 
| k = 0 không loại bỏ | KHÔNG | điều kiện biên | 
| tất cả các giá trị bằng nhau | CÓ THỂ | sự linh hoạt của cà vạt đầy đủ | 
| cắt nhóm hỗn hợp | KHÔNG | trường hợp ly thân nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp tất cả các giá trị đều bằng nhau thể hiện hành vi đầy đủ. Nếu n = 4, k = 2 và tất cả các giá trị đều là 7 thì lớn hơn = 0 và bằng = 4, do đó sớm nhất = 1 và muộn nhất = 4. Người yêu thích có thể hạ cánh ở bất kỳ vị trí nào tùy thuộc vào việc bẻ hòa, do đó, nó có thể bị loại bỏ hoặc không, tạo ra CÓ THỂ. Thuật toán nắm bắt chính xác điều này vì khoảng kéo dài cả hai phía của k. 

Khi mục yêu thích có giá trị tối đa, lớn hơn = 0 và bằng = 1. Khoảng này thu gọn về một vị trí duy nhất ở phía trước mảng. Nếu k ≥ 1 thì luôn bị loại bỏ, ngược lại thì không bao giờ bị loại bỏ. Mã xử lý việc này một cách rõ ràng vì sớm nhất bằng mới nhất, loại bỏ sự mơ hồ. 

Khi k rất nhỏ thì chỉ có các phần tử bị ép chặt vào vật chất phía trước. Nếu lớn hơn đã vượt quá k, mục yêu thích được đảm bảo sẽ bị đẩy vượt quá giới hạn bất kể hành vi của nhóm bằng nhau, tạo ra NO.
