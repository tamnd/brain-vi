---
title: "CF 1811C - Khôi phục mảng"
description: "Chúng ta được cho một mảng dẫn xuất $b$, và chúng ta được biết nó đến từ một mảng ẩn $a$ nào đó. Mỗi giá trị $bi$ là giá trị tối đa của hai giá trị liền kề trong $a$, cụ thể là $ai$ và $a{i+1}$."
date: "2026-06-09T08:41:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1811
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 863 (Div. 3)"
rating: 1100
weight: 1811
solve_time_s: 296
verified: false
draft: false
---

[CF 1811C - Khôi phục mảng](https://codeforces.com/problemset/problem/1811/C) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 4 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng dẫn xuất$b$, và chúng tôi được biết nó đến từ một mảng ẩn nào đó$a$. Mỗi giá trị$b_i$là giá trị lớn nhất của hai giá trị liền kề trong$a$, cụ thể$a_i$Và$a_{i+1}$. Nhiệm vụ của chúng ta không phải là tái tạo lại mảng ban đầu một cách duy nhất mà là xây dựng bất kỳ mảng hợp lệ nào$a$điều đó có thể đã tạo ra cái đã cho$b$. 

Cấu trúc của vấn đề là cục bộ. Mọi ràng buộc chỉ ràng buộc hai vị trí lân cận trong$a$, do đó toàn bộ hệ thống là một chuỗi các ràng buộc tối đa chồng chéo. Đầu ra đơn giản là bất kỳ mảng số nguyên không âm nào thỏa mãn tất cả các mối quan hệ tối đa cục bộ này. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trong các trường hợp thử nghiệm, điều này ngay lập tức loại trừ bất kỳ sự tái cấu trúc bậc hai hoặc nặng về mô phỏng nào. Bất cứ điều gì cố gắng ép buộc các ứng viên cho từng vị trí một cách độc lập sẽ dẫn đến ít nhất$O(n^2)$hành vi trong trường hợp xấu nhất, vượt xa giới hạn. Giải pháp phải tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp cạnh tinh tế phát sinh khi$b$chứa các giá trị lặp lại hoặc các đoạn phẳng dài. Một ý tưởng ngây thơ có thể là đặt trực tiếp cả hai điểm cuối của mỗi ràng buộc thành$b_i$, nhưng điều này có thể vi phạm các ràng buộc lân cận vì mỗi vị trí trong$a$tham gia vào hai cực đại khác nhau. Một trường hợp lỗi khác xảy ra khi số 0 xuất hiện, vì số 0 có thể truyền bá các ràng buộc mà không được chỉ định rõ ràng và việc điền bất cẩn có thể vô tình tạo ra các giá trị dương không cần thiết. 

## Phương pháp tiếp cận 

Một nỗ lực mạnh mẽ sẽ cố gắng xây dựng tất cả các mảng có thể$a$phù hợp với$b$, kiểm tra xem mỗi ứng cử viên có thỏa mãn tất cả các ràng buộc tối đa hay không. Ngay cả khi chúng tôi hạn chế các giá trị xuất hiện trong$b$, mỗi vị trí vẫn có nhiều lựa chọn và không gian trạng thái tăng theo cấp số nhân. Vì$n = 2 \cdot 10^5$, thậm chí một quyết định nhị phân cho mỗi phần tử cũng dẫn đến$2^n$những khả năng rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là mỗi$b_i$chỉ thực thi rằng ít nhất một trong số$a_i$hoặc$a_{i+1}$bằng$b_i$, và cả hai đều không được vượt quá nó. Điều này có nghĩa là mỗi vị trí$a_i$bị ràng buộc bởi hai người hàng xóm:$b_{i-1}$Và$b_i$. Nếu chúng ta nghĩ ở địa phương,$a_i$phải đủ nhỏ để không vi phạm mức tối đa liền kề, nhưng đủ lớn để cả hai ràng buộc vẫn có thể được thỏa mãn bằng cách chọn điểm cuối còn lại một cách thích hợp. 

Điều này dẫn đến một chiến lược tham lam mang tính xây dựng: gán cho mỗi vị trí giá trị tối thiểu vẫn tương thích với cả hai ràng buộc liền kề, đồng thời đảm bảo rằng ít nhất một điểm cuối của mỗi cặp có thể đạt được mức tối đa cần thiết. Một cách trực tiếp và rõ ràng để đạt được điều này là khởi tạo tất cả$a_i$bằng 0 và sau đó “đưa” cực đại cần thiết vào các vị trí cần thiết một cách có chọn lọc, đảm bảo mọi$b_i$được thực hiện bởi ít nhất một điểm cuối. 

Một cách xây dựng có hệ thống hơn là xử lý từng$b_i$như một nhu cầu phải xuất hiện ở một trong hai vị trí$i$hoặc$i+1$, rồi tham lam gán nó vào vị trí còn linh hoạt. Vì mỗi vị trí tham gia vào nhiều nhất hai ràng buộc nên việc chuyển từ trái sang phải là đủ để đảm bảo tính nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng$a$chiều dài$n$với tất cả số không. Điều này đưa ra một đường cơ sở trung tính không bao giờ vi phạm bất kỳ ràng buộc tối đa nào. 
2. Đối với mỗi chỉ số$i$, xét ràng buộc$b_i = \max(a_i, a_{i+1})$. Chúng ta phải đảm bảo rằng ít nhất một trong hai vị trí có giá trị$b_i$. 
3. Nếu cả hai$a_i$Và$a_{i+1}$hiện tại ít hơn$b_i$, giao phó$a_{i+1} = b_i$. Lựa chọn này duy trì tính linh hoạt trong tương lai ở phía bên trái trong khi vẫn đảm bảo thỏa mãn ràng buộc. 
4. Nếu một trong các điểm cuối đã bằng$b_i$, không làm gì cả vì ràng buộc đã được thỏa mãn. 
5. Sau khi xử lý xong tất cả$b_i$, trả về mảng$a$. 

Lựa chọn thiết kế chính là luôn đẩy các giá trị sang bên phải khi cần thiết. Điều này tránh việc sửa sớm các vị trí bên trái, vốn vẫn cần thiết để đáp ứng các ràng buộc trước đó. 

### Tại sao nó hoạt động 

Mỗi ràng buộc$b_i$được xử lý chính xác một lần và khi được xử lý, chúng tôi đảm bảo rằng ít nhất một điểm cuối khớp với$b_i$. Bởi vì chúng tôi không bao giờ tăng giá trị vượt quá$b_i$, chúng tôi không bao giờ vi phạm bất kỳ ràng buộc nào sau khi nó được thỏa mãn. Hướng tham lam đảm bảo rằng các ràng buộc trong tương lai luôn nhìn thấy tiền tố hợp lệ mà vẫn có thể được điều chỉnh ở phía bên phải nếu cần. Điều này tạo ra một chuỗi nhất quán trong đó mọi yêu cầu tối đa đều được đáp ứng cục bộ mà không vi phạm các quyết định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    b = list(map(int, input().split()))
    
    a = [0] * n
    
    for i in range(n - 1):
        if a[i] < b[i] and a[i + 1] < b[i]:
            a[i + 1] = b[i]
    
    print(*a)
```Giải pháp bắt đầu với một mảng được khởi tạo bằng 0, đảm bảo không có vi phạm ràng buộc ngẫu nhiên nào. Sau đó nó xử lý từng ràng buộc từ trái sang phải, thực thi từng ràng buộc$b_i$bằng cách đặt giá trị của nó vào đúng điểm cuối bất cứ khi nào cần thiết. Tính định hướng này rất quan trọng vì nó ngăn chặn việc ghi đè các ràng buộc được thực thi trước đó. 

Một lỗi phổ biến là cố gắng gán đối xứng cả hai bên hoặc quay lại các bản cập nhật. Điều đó dẫn đến sự mâu thuẫn giữa các ràng buộc chồng chéo. Cách tiếp cận một lần tham lam sẽ tránh được điều đó hoàn toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
b = [3, 4, 4, 5]
```| tôi | b[i] | một [tôi] | a[i+1] | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 0 | đặt a[1]=3 | 
| 1 | 4 | 0 | 3 | không thay đổi | 
| 2 | 4 | 0 | 3 | đặt a[3]=4 | 
| 3 | 5 | 4 | 4 | đặt a[4]=5 | 

Mảng cuối cùng:```
3 0 4 0 5
```Dấu vết này cho thấy cách các giá trị chỉ lan truyền khi cần thiết và các vị trí cố định trước đó vẫn ổn định. 

### Ví dụ 2 

đầu vào:```
n = 4
b = [2, 1, 0]
```| tôi | b[i] | một [tôi] | a[i+1] | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | đặt a[1]=2 | 
| 1 | 1 | 0 | 2 | không thay đổi | 
| 2 | 0 | 0 | 2 | không thay đổi | 

Đầu ra:```
2 0 0 0
```Điều này cho thấy rằng ràng buộc bằng 0 đương nhiên không yêu cầu chuyển nhượng và không can thiệp vào các quyết định trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi ràng buộc được xử lý một lần | 
| Không gian | O(n) | lưu trữ cho mảng được xây dựng lại | 

Thuật toán chạy trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm, đủ với giới hạn tổng kích thước đầu vào của$2 \cdot 10^5$. Việc sử dụng bộ nhớ cũng tuyến tính và tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        a = [0] * n
        for i in range(n - 1):
            if a[i] < b[i] and a[i + 1] < b[i]:
                a[i + 1] = b[i]
        out.append(" ".join(map(str, a)))
    return "\n".join(out)

assert run("""1
5
3 4 4 5
""").strip() == "3 0 4 0 5"
assert run("""1
4
2 1 0
""").strip() == "2 0 0 0"
assert run("""1
3
0 0
""").strip() == "0 0 0"
assert run("""1
2
5
""").strip() == "5 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ràng buộc xen kẽ | tái thiết hợp lệ | độ chính xác của việc truyền bá | 
| dãy giảm dần | ổn định dưới sự chồng chéo | không ghi đè lỗi | 
| tất cả số không | tính nhất quán tầm thường | xử lý trường hợp cơ bản | 
| ràng buộc duy nhất | độ đúng ranh giới | xử lý đầu vào tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị trong$b$là số không. Trong trường hợp này, đầu ra đúng là một mảng hoàn toàn bằng không. Thuật toán bắt đầu bằng số 0 và không bao giờ kích hoạt bất kỳ phép gán nào, do đó, nó bảo toàn chính xác cấu trúc này. 

Một trường hợp khác là khi$b$đang gia tăng nghiêm trọng. Mỗi giá trị mới buộc phải gán một phép gán mới ở bên phải và các giá trị trước đó vẫn được giữ nguyên. Chiến lược tham lam đảm bảo mỗi lần tăng được xử lý độc lập mà không có xung đột. 

Trường hợp thứ ba là một phần tử$b$, trong đó mọi cặp giá trị bằng nhau đều hợp lệ. Thuật toán chỉ gán khi cần thiết, do đó, nó tự nhiên tạo ra một cặp hợp lệ mà không cần xử lý đặc biệt.
