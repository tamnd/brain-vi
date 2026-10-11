---
title: "CF 105009A - Mảng TriNum"
description: "Chúng ta được cung cấp một chuỗi các số nguyên được trình bày trên một dòng và chúng ta được yêu cầu tìm một đoạn liền kề của chuỗi này sử dụng tối đa ba giá trị riêng biệt, càng dài càng tốt. Hãy coi mảng này như một dòng thời gian của các giao dịch ở Numerica."
date: "2026-06-28T03:02:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105009
codeforces_index: "A"
codeforces_contest_name: "2024 USACO.Guide Informatics Tournament"
rating: 0
weight: 105009
solve_time_s: 68
verified: true
draft: false
---

[CF 105009A - Mảng ba số](https://codeforces.com/problemset/problem/105009/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên được trình bày trên một dòng và chúng ta được yêu cầu tìm một đoạn liền kề của chuỗi này sử dụng tối đa ba giá trị riêng biệt, càng dài càng tốt. 

Hãy coi mảng này như một dòng thời gian của các giao dịch ở Numerica. Mỗi số là một loại mặt hàng đang được giao dịch tại thời điểm đó. Chúng tôi muốn chọn một khoảng thời gian liên tục mà thị trường chỉ giao dịch tối đa ba loại mặt hàng. Trong số tất cả các đoạn trải dài hợp lệ như vậy, chúng ta muốn đoạn có độ dài tối đa. 

Đầu ra chỉ là một con số, độ dài của đoạn kéo dài tốt nhất có thể. 

Kích thước mảng có thể lên tới 100000, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra rõ ràng mọi mảng con. Cách tiếp cận bậc hai hoặc bậc ba sẽ yêu cầu thứ tự các phép toán 10¹⁰ trong trường hợp xấu nhất, vượt xa những gì chạy trong một giây. Điều này đẩy chúng ta tới các kỹ thuật tuyến tính hoặc gần tuyến tính, thường liên quan đến một cửa sổ trượt hoặc hai con trỏ. 

Một vấn đề tế nhị nảy sinh khi nghĩ đến việc bành trướng tham lam. Nếu chúng ta mở rộng một phân đoạn cho đến khi nó vi phạm ràng buộc, chúng ta phải cẩn thận về cách thu nhỏ phân đoạn đó. Một cách tiếp cận ngây thơ khởi động lại từ đầu sau mỗi lần vi phạm có thể bỏ lỡ các cửa sổ tối ưu hoặc trở nên quá chậm. 

Ví dụ, hãy xem xét một mảng như`[1, 2, 3, 4, 1, 2, 3]`. Phân đoạn tối ưu là bất kỳ cửa sổ nào chỉ chứa ba giá trị riêng biệt, chẳng hạn như`[1, 2, 3, 4]`không hợp lệ, nhưng`[2, 3, 4, 1]`cũng không hợp lệ do bốn giá trị riêng biệt. Câu trả lời đúng là 3 từ các phân đoạn như`[1, 2, 3]`. Nếu chúng tôi khởi động lại không chính xác bất cứ khi nào chúng tôi thấy giá trị riêng biệt thứ tư, chúng tôi có thể liên tục loại bỏ sự trùng lặp hữu ích và tính toán lại một cách không cần thiết. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Chúng tôi xem xét mọi chỉ mục bắt đầu và mở rộng mảng con sang bên phải đồng thời theo dõi số lượng số riêng biệt xuất hiện. Đối với mỗi lần bắt đầu, chúng tôi mở rộng cho đến khi đạt được bốn giá trị riêng biệt và ghi lại độ dài hợp lệ tốt nhất được thấy trước thời điểm đó. 

Điều này hiệu quả vì nó kiểm tra rõ ràng mọi phân khúc ứng cử viên. Vấn đề là chi phí của nó. Đối với mỗi vị trí bắt đầu trong số N vị trí bắt đầu, chúng tôi có thể quét tối đa N phần tử và việc duy trì số lượng riêng biệt theo cách đơn giản sẽ bổ sung thêm chi phí. Ngay cả khi băm, độ phức tạp trong trường hợp xấu nhất là O(N²), dẫn đến khoảng 10¹⁰ phép toán khi N = 10⁵, rõ ràng là quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần phải khởi động lại quá trình quét từ mỗi chỉ mục. Khi chúng ta di chuyển con trỏ bên phải về phía trước, con trỏ bên trái cũng chỉ cần di chuyển về phía trước để khôi phục ràng buộc của tối đa ba giá trị riêng biệt. Khi một giá trị hoàn toàn rời khỏi cửa sổ, giá trị đó sẽ bị xóa khỏi cấu trúc tần số của chúng tôi. Chuyển động đơn điệu này của cả hai con trỏ đảm bảo rằng mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần. 

Đây chính xác là kỹ thuật cửa sổ trượt: chúng tôi duy trì một cửa sổ luôn hợp lệ (nhiều nhất là ba giá trị riêng biệt) và chúng tôi mở rộng nó một cách tham lam trong khi sửa các vi phạm bằng cách thu nhỏ từ bên trái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) đến O(N) | Quá chậm | 
| Cửa Sổ Trượt Tối Ưu | O(N) | O(1) đến O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cửa sổ`[l, r]`và bản đồ tần số của các giá trị bên trong nó. 

1. Khởi tạo`l = 0`,`best = 0`và một bản đồ tần số trống. 

Điều này thiết lập một cửa sổ trượt trống. 
2. Di chuyển con trỏ sang phải`r`từ trái sang phải trên mảng. 

Mỗi bước thêm một phần tử mới vào cửa sổ hiện tại, khám phá dần dần tất cả các mảng con kết thúc tại`r`. 
3. Khi thêm`A[r]`, tăng tần suất của nó trên bản đồ. 

Điều này giúp đếm chính xác số lần mỗi giá trị xuất hiện bên trong cửa sổ hiện tại. 
4. Nếu số lượng khóa riêng biệt trong bản đồ lớn hơn 3, hãy thu nhỏ cửa sổ từ bên trái. 

Chúng tôi liên tục giảm tần số của`A[l]`và di chuyển`l`chuyển tiếp cho đến khi cửa sổ lại chứa tối đa ba giá trị riêng biệt. Điều này là cần thiết vì bất kỳ cửa sổ không hợp lệ nào cũng không thể đóng góp vào câu trả lời. 
5. Sau khi khôi phục tính hợp lệ, hãy cập nhật câu trả lời bằng`best = max(best, r - l + 1)`. 

Tại thời điểm này,`[l, r]`là mảng con hợp lệ dài nhất kết thúc tại`r`, vì vậy có thể an toàn khi xem xét nó ở mức tối đa toàn cục. 
6. Tiếp tục cho đến khi`r`đến cuối mảng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một bất biến khóa: tại mọi vị trí của`r`, cửa sổ`[l, r]`chứa tối đa ba giá trị phân biệt và`l`là chỉ mục nhỏ nhất duy trì thuộc tính này cho hiện tại`r`. Bởi vì chúng ta chỉ di chuyển`l`chuyển tiếp khi cần thiết, chúng tôi không bao giờ loại bỏ thời hạn có khả năng tốt hơn kết thúc tại`r`. Mỗi mảng con hợp lệ được biểu diễn tại một số điểm dưới dạng điểm cuối cửa sổ và mức tối đa trên các mảng này là câu trả lời chung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    l = 0
    best = 0
    
    for r in range(n):
        freq[a[r]] = freq.get(a[r], 0) + 1
        
        while len(freq) > 3:
            freq[a[l]] -= 1
            if freq[a[l]] == 0:
                del freq[a[l]]
            l += 1
        
        best = max(best, r - l + 1)
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng trực tiếp xung quanh cửa sổ trượt bất biến. Từ điển`freq`theo dõi sự đa dạng và điều kiện`len(freq) > 3`phát hiện khi cửa sổ vi phạm ràng buộc. 

Vòng lặp while bên trong rất quan trọng: nó đảm bảo cửa sổ luôn được sửa chữa ngay sau khi vi phạm. Không xóa các phím khi tần số của chúng giảm xuống 0,`len(freq)`sẽ không chính xác, đó là nguồn lỗi phổ biến. 

biểu hiện`r - l + 1`tính toán kích thước cửa sổ hiện tại và biểu thị mảng con hợp lệ tốt nhất kết thúc tại`r`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 4 2 1 9 1 10
```Chúng tôi theo dõi`(l, r, window, freq, best)`: 

| r | tôi | cửa sổ | tần số | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | {1:1} | 1 | 
| 1 | 0 | [1,4] | {1:1,4:1} | 2 | 
| 2 | 0 | [1,4,2] | {1:1,4:1,2:1} | 3 | 
| 3 | 0 | [1,4,2,1] | {1:2,4:1,2:1} | 4 | 
| 4 | 1 | [4,2,1,9] → thu nhỏ | {2:1,1:1,9:1} | 4 | 
| 5 | 2 | [2,1,9,1] | {2:1,1:2,9:1} | 4 | 
| 6 | 3 | cửa sổ co lại nhiều lần | cửa sổ hợp lệ | 4 | 

Điều này cho thấy rằng khi giá trị phân biệt thứ tư xuất hiện, con trỏ bên trái sẽ di chuyển về phía trước cho đến khi chỉ còn lại ba giá trị và đoạn hợp lệ tốt nhất được tìm thấy là độ dài 4. 

### Ví dụ 2 

đầu vào:```
6
1 2 3 2 1 2
```| r | tôi | cửa sổ | tần số | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | {1:1} | 1 | 
| 1 | 0 | [1,2] | {1:1,2:1} | 2 | 
| 2 | 0 | [1,2,3] | {1:1,2:1,3:1} | 3 | 
| 3 | 0 | [1,2,3,2] | {1:1,2:2,3:1} | 4 | 
| 4 | 0 | [1,2,3,2,1] | {1:2,2:2,3:1} | 5 | 
| 5 | 0 | [1,2,3,2,1,2] | {1:2,2:3,3:1} | 6 | 

Ở đây chúng tôi không bao giờ vượt quá ba giá trị riêng biệt, vì vậy toàn bộ mảng là hợp lệ. 

Những dấu vết này xác nhận rằng thuật toán liên tục duy trì tính hợp lệ trong khi mở rộng hết mức có thể trước khi thu hẹp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | mỗi phần tử vào và ra khỏi cửa sổ nhiều nhất một lần | 
| Không gian | O(1) đến O(N) | bản đồ tần số lưu trữ tối đa 3 phím trong cửa sổ | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc đối với N lên tới 100000 và mức sử dụng bộ nhớ vẫn ở mức nhỏ do nhiều nhất một số khóa được lưu trữ bất kỳ lúc nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        l = 0
        best = 0
        
        for r in range(n):
            freq[a[r]] = freq.get(a[r], 0) + 1
            while len(freq) > 3:
                freq[a[l]] -= 1
                if freq[a[l]] == 0:
                    del freq[a[l]]
                l += 1
            best = max(best, r - l + 1)
        
        return best
    
    return str(solve())

assert run("7\n1 4 2 1 9 1 10\n") == "4"
assert run("6\n1 2 3 2 1 2\n") == "6"
assert run("1\n5\n") == "1"
assert run("5\n1 1 1 1 1\n") == "5"
assert run("6\n1 2 3 4 5 6\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | kích thước tối thiểu | 
| tất cả đều bình đẳng | n | không cần thu nhỏ | 
| tất cả đều khác biệt | 3 | hành vi thu hẹp thường xuyên | 
| trường hợp hỗn hợp | mở rộng cửa sổ đúng cách | tính đúng đắn chung | 

## Vỏ cạnh 

Một đầu vào tối thiểu như`1`kiểm tra xem thuật toán có khởi tạo chính xác câu trả lời mà không cần vào vòng lặp hay không. Cửa sổ bắt đầu và kết thúc tại một phần tử duy nhất và bản đồ tần số chứa một phím, tạo ra đầu ra 1 ngay lập tức. 

Một mảng hoàn toàn thống nhất như`5 5 5 5 5`không bao giờ gây ra tình trạng co lại. Bản đồ tần số luôn có kích thước 1, do đó cửa sổ phát triển đơn điệu đến hết chiều dài. Điều này xác nhận rằng thuật toán không thu nhỏ các cửa sổ hợp lệ một cách giả tạo. 

Một mô hình xen kẽ nghiêm ngặt như`1 2 3 4 5 6`lực liên tục co lại. Khi giá trị phân biệt thứ tư xuất hiện, con trỏ bên trái sẽ tiến lên cho đến khi chỉ còn lại ba giá trị. Điều này kiểm tra tính chính xác của logic xóa khi tần số giảm xuống 0 và đảm bảo rằng việc kiểm tra số lượng khác biệt vẫn chính xác trong suốt quá trình thực thi.
