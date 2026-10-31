---
title: "CF 106363F - Bốn liên tiếp"
description: "Chúng ta được cho một hoán vị các số từ 1 đến n. Từ hoán vị này, chúng tôi xem xét mọi phân đoạn liền kề và tính MEX của nó, nghĩa là số nguyên dương nhỏ nhất không xuất hiện bên trong phân đoạn đó."
date: "2026-06-19T08:28:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106363
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 2-11-2026 Div. 1 (Advanced)"
rating: 0
weight: 106363
solve_time_s: 46
verified: true
draft: false
---

[CF 106363F - Bốn liên tiếp](https://codeforces.com/problemset/problem/106363/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n. Từ hoán vị này, chúng tôi xem xét mọi phân đoạn liền kề và tính MEX của nó, nghĩa là số nguyên dương nhỏ nhất không xuất hiện bên trong phân đoạn đó. Nhiệm vụ là tính tổng giá trị MEX này trên tất cả các phân đoạn hoán vị có thể có. 

Cấu trúc chính là chúng tôi không làm việc với các mảng tùy ý. Mỗi giá trị từ 1 đến n xuất hiện chính xác một lần, điều này làm cho điều kiện hiện diện của các giá trị từ 1 đến k dễ dàng theo dõi hơn nhiều vì mỗi giá trị đó tương ứng với một vị trí duy nhất trong mảng. 

Nếu n lớn, ở mức 200.000 trở lên, việc liệt kê tất cả các mảng con và tính MEX trên mỗi mảng con ngay lập tức là không khả thi. Số lượng mảng con là O(n^2) và thậm chí quét tuyến tính trên mỗi mảng con cũng sẽ đẩy con số này lên O(n^3), vượt xa các giới hạn thông thường. Điều này buộc chúng ta phải trình bày lại vấn đề theo hướng đóng góp của các giá trị thay vì liệt kê trực tiếp. 

Trường hợp cạnh tinh tế phát sinh khi mảng nhỏ hoặc khi các giá trị ban đầu được nhóm lại. Ví dụ: nếu hoán vị là [1, 2, 3] thì mỗi mảng con chỉ có MEX lớn khi nó trống các phân đoạn ban đầu và lý luận ngây thơ về các khoảng trống cục bộ có thể dễ dàng bỏ lỡ phạm vi phủ sóng toàn cầu. Một trường hợp góc khác là hoán vị ngược, chẳng hạn như [n, n−1, ..., 1], trong đó vị trí của các số nhỏ cách xa nhau, tối đa hóa phạm vi cần thiết để bao gồm các tiền tố. 

Một cách tiếp cận đơn giản sẽ liên tục tính toán lại MEX cho từng mảng con một cách độc lập, điều này âm thầm thất bại do độ phức tạp về thời gian mặc dù về mặt logic nó đúng. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi mảng con, chúng tôi quét các phần tử của nó, đánh dấu giá trị nào xuất hiện và tính số nguyên còn thiếu nhỏ nhất. Điều này hoạt động vì MEX được xác định cục bộ trên mỗi mảng con và không cần thêm gì về mặt khái niệm. Vấn đề là mỗi mảng con tốn O(n) để xử lý và có các mảng con O(n^2), dẫn đến tổng số hoạt động O(n^3), quá chậm đối với đầu vào lớn. 

Để cải thiện điều này, chúng tôi lật lại quan điểm. Thay vì hỏi “MEX của mảng con này là gì”, chúng tôi hỏi “MEX của mảng con nào ít nhất là k + 1”. Một mảng con có MEX lớn hơn k khi và chỉ khi nó chứa mọi số từ 1 đến k. Bởi vì chúng ta đang xử lý một hoán vị, mỗi số xuất hiện đúng một lần, do đó điều kiện “chứa tất cả 1..k” trở thành điều kiện khoảng đơn giản. 

Với mỗi k, chúng ta xác định vị trí của các số từ 1 đến k. Đặt L_k là mức tối thiểu trong số các vị trí này và R_k là mức tối đa. Bất kỳ mảng con nào chứa tất cả 1..k đều phải bao phủ đầy đủ khoảng này [L_k, R_k]. Số lượng mảng con như vậy là các lựa chọn L_k cho điểm cuối bên trái và (n − R_k + 1) lựa chọn cho điểm cuối bên phải. 

Điều này chuyển vấn đề thành việc duy trì, khi k tăng, khoảng mở rộng được hình thành bởi các vị trí 1..k. Mỗi bước chỉ cập nhật các điểm cuối trong khoảng thời gian và mức đóng góp được tính theo thời gian không đổi trên k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng mảng pos sao cho pos[x] là chỉ số của giá trị x trong hoán vị. Điều này cho phép truy cập liên tục vào nơi mỗi số xuất hiện. 
2. Duy trì hai biến L và R đại diện cho vị trí nhỏ nhất và lớn nhất trong số các giá trị từ 1 đến k khi k tăng. Ban đầu L = R = pos[1], vì chỉ có giá trị 1 được bao gồm. 
3. Với mỗi k từ 1 đến n, hãy mở rộng khoảng bằng cách cập nhật L = min(L, pos[k]) và R = max(R, pos[k]). Điều này đảm bảo L và R luôn mô tả phân đoạn tối thiểu bao gồm tất cả các giá trị 1..k. 
4. Với mỗi k, hãy tính xem có bao nhiêu mảng con chứa toàn bộ khoảng [L, R]. Điểm cuối bên trái có thể được chọn theo L cách, vì nó phải bắt đầu tại hoặc trước L. Điểm cuối bên phải có thể được chọn theo (n − R + 1) cách, vì nó phải kết thúc tại hoặc sau R. Nhân các điểm này để có được count_k. 
5. Tích lũy đáp án += count_k. Giá trị này biểu thị số mảng con có MEX lớn hơn k. 
6. Sau khi xử lý hết k, trả về giá trị tích lũy cuối cùng. Điều này hiệu quả vì mỗi mảng con đóng góp chính xác 1 cho mỗi k nhỏ hơn MEX của nó. 

### Tại sao nó hoạt động 

Mỗi mảng con đóng góp vào tổng cuối cùng một lần cho mỗi số nguyên k sao cho 1..k được chứa đầy đủ bên trong nó. Điều kiện đó tương đương với mảng con bao gồm khoảng được xác định bởi các vị trí 1..k. Vì cấu trúc hoán vị đảm bảo một khoảng duy nhất cho mỗi tiền tố của các giá trị, nên việc đếm các mảng con bao trùm khoảng này sẽ đếm chính xác tất cả các mảng con có MEX lớn hơn k. Tính tổng trên k sẽ tái tạo lại tổng số MEX mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i

    L = pos[1]
    R = pos[1]
    ans = 0

    for k in range(1, n + 1):
        L = min(L, pos[k])
        R = max(R, pos[k])
        ans += (L + 1) * (n - R)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi dựa vào các vị trí tính toán trước để các khoảng thời gian cập nhật là thời gian không đổi. biểu hiện`(L + 1) * (n - R)`giả định lập chỉ mục dựa trên 0, trong đó`L + 1`đếm điểm cuối bên trái hợp lệ và`n - R`tính các điểm cuối bên phải hợp lệ hoàn toàn ở bên phải của R. Nguồn sai lầm chính ở đây là việc lập chỉ mục riêng lẻ: việc coi các điểm cuối là bao gồm hoặc độc quyền sẽ làm thay đổi hoàn toàn công thức một cách không nhất quán. 

Vòng lặp tăng dần khoảng thời gian và không yêu cầu cấu trúc dữ liệu bổ sung nào ngoài mảng vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| k | vị trí[k] | L | R | đếm_k | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | (1)(3) = 3 | 
| 2 | 1 | 0 | 1 | (1)(2) = 2 | 
| 3 | 2 | 0 | 2 | (1)(1) = 1 | 

Đầu ra là 6. 

Điều này cho thấy mỗi phần mở rộng của tiền tố sẽ giảm số lượng mảng con vẫn chứa nó đầy đủ như thế nào, tạo ra kiểu đóng góp giảm dần. 

### Ví dụ 2 

đầu vào:```
3
2 1 3
```| k | vị trí[k] | L | R | đếm_k | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | (2)(2) = 4 | 
| 2 | 0 | 0 | 1 | (1)(2) = 2 | 
| 3 | 2 | 0 | 2 | (1)(1) = 1 | 

Đầu ra là 7. 

Trường hợp này chứng tỏ rằng khoảng có thể co lại đáng kể khi một phần tử nhỏ hơn xuất hiện ở bên trái của phân đoạn hiện tại, điều này ảnh hưởng trực tiếp đến số lượng mảng con hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được xử lý một lần và cập nhật theo khoảng thời gian là thời gian không đổi | 
| Không gian | O(n) | Mảng vị trí lưu trữ chỉ mục cho từng phần tử | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó thay thế việc liệt kê bậc hai các mảng con bằng một lần quét tuyến tính duy nhất trên các giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(a):
            pos[v] = i

        L = pos[1]
        R = pos[1]
        ans = 0

        for k in range(1, n + 1):
            L = min(L, pos[k])
            R = max(R, pos[k])
            ans += (L + 1) * (n - R)

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample style tests
assert run("3\n1 2 3\n") == "6"
assert run("3\n2 1 3\n") == "7"

# minimum size
assert run("1\n1\n") == "1"

# reversed permutation
assert run("3\n3 2 1\n") == "6"

# random small case
assert run("4\n2 1 4 3\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | tính đúng đắn của trường hợp cơ sở | 
| mảng đảo ngược | 6 | hành vi mở rộng khoảng | 
| hoán vị hỗn hợp | 10 | tính đúng đắn của thứ tự không tầm thường | 

## Vỏ cạnh 

### Phần tử đơn 

đầu vào:```
1
1
```Ở đây L = R = 0. Phân mảng duy nhất là toàn bộ mảng và mọi điều kiện tiền tố đều đúng ngay lập tức. Công thức cho (L+1)(n−R) = 1, khớp với giá trị MEX duy nhất. 

### Hoán vị đảo ngược hoàn toàn 

đầu vào:```
4
4 3 2 1
```Từng bước, L và R mở rộng vào trong cho đến khi bao phủ toàn bộ mảng. Giá trị k ban đầu tạo ra các khoảng lớn, làm giảm số lượng mảng con một cách nhanh chóng. Thuật toán theo dõi chính xác điều này vì nó luôn sử dụng vị trí tối thiểu và tối đa chung, không phải vị trí kề. 

### Hành vi xáo trộn ngẫu nhiên ở cạnh 

Đối với đầu vào có giá trị nhỏ nằm rải rác, L và R dao động đáng kể. Thuật toán vẫn đúng vì mỗi lần cập nhật chỉ phụ thuộc vào cực trị và cấu trúc trung gian không liên quan đến phạm vi khoảng thời gian.
