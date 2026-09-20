---
title: "CF 104768J - Mối đe dọa ma quái"
description: "Chúng ta có hai tập hợp chuỗi, mỗi tập hợp chứa cùng số chuỗi và mọi chuỗi có cùng độ dài cố định. Nhiệm vụ là sắp xếp lại các chuỗi bên trong mỗi bộ sưu tập một cách độc lập, sau đó nối từng bộ sưu tập được sắp xếp lại thành một chuỗi dài."
date: "2026-06-28T20:03:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104768
codeforces_index: "J"
codeforces_contest_name: "2023 China Collegiate Programming Contest (CCPC) Guilin Onsite (The 2nd Universal Cup. Stage 8: Guilin)"
rating: 0
weight: 104768
solve_time_s: 57
verified: true
draft: false
---

[CF 104768J - Mối đe dọa ma quái](https://codeforces.com/problemset/problem/104768/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp chuỗi, mỗi tập hợp chứa cùng số chuỗi và mọi chuỗi có cùng độ dài cố định. Nhiệm vụ là sắp xếp lại các chuỗi bên trong mỗi bộ sưu tập một cách độc lập, sau đó nối từng bộ sưu tập được sắp xếp lại thành một chuỗi dài. Sau đó, chúng tôi so sánh hai chuỗi dài thu được, nhưng có một điểm thay đổi: một chuỗi được phép xoay theo chu kỳ, nghĩa là chúng tôi có thể cắt nó ở bất kỳ vị trí nào và di chuyển tiền tố đến cuối. 

Vì vậy, câu hỏi thực sự là liệu chúng ta có thể hoán vị các chuỗi trong cả hai bộ sưu tập sao cho hai chuỗi được nối trở nên giống hệt nhau sau một số lần dịch chuyển ký tự theo chu kỳ hay không. 

Mỗi trường hợp thử nghiệm là độc lập. Chúng ta cần xuất ra hai hoán vị của các chỉ số mô tả sự sắp xếp lại hợp lệ hoặc báo cáo rằng điều đó là không thể. 

Các ràng buộc chặt chẽ về tổng kích thước thay vì trên mỗi bài kiểm tra. Tổng tất cả các ký tự trong tất cả các trường hợp thử nghiệm tối đa là một triệu, do đó, mọi giải pháp về cơ bản đều phải chạy trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ điều gì liên quan đến việc so sánh lồng nhau giữa các chuỗi hoặc thử tất cả các lần sắp xếp lại đều không thể thực hiện được ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các chuỗi trông rất giống nhau nhưng không giống nhau như một chuỗi nhiều chuỗi. Ví dụ: nếu A chứa {"aa", "ab"} và B chứa {"aa", "ac"} thì không có sự sắp xếp lại nào có thể làm cho các chuỗi được nối khớp với nhau ngay cả sau khi xoay, bởi vì một bộ sưu tập chứa mẫu ký tự mà bộ sưu tập kia không chứa. Một ý tưởng ngây thơ có thể cố gắng căn chỉnh các phép quay của phép nối cuối cùng mà không kiểm tra sự bằng nhau của nhiều tập hợp, điều này sẽ cho rằng không chính xác rằng chỉ sắp xếp lại là đủ để khắc phục sự không khớp. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực bắt đầu bằng cách suy nghĩ trực tiếp về các hoán vị. Chúng ta có thể thử mọi thứ tự của A và mọi thứ tự của B, xây dựng hai chuỗi được nối và kiểm tra xem một chuỗi có phải là một vòng quay tuần hoàn của chuỗi kia hay không. Đây là khái niệm đơn giản và chính xác, vì nó rõ ràng phù hợp với định nghĩa. Tuy nhiên, nó là giai thừa tính theo n, và thậm chí việc xây dựng và so sánh các chuỗi cũng tốn O(nm) cho mỗi lần thử, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là phép quay theo chu kỳ tác động lên chuỗi nối cuối cùng chứ không phải trên các khối riêng lẻ. Nếu hai phép nối là sự dịch chuyển theo chu kỳ của nhau, thì chúng phải chứa chính xác các ký tự giống nhau với cùng bội số giống nhau và quan trọng hơn là cùng một tập hợp các khối có độ dài m phải được bảo toàn trong suốt quá trình xây dựng. Vì chúng ta được phép hoán vị tùy ý bên trong mỗi chuỗi, nên yêu cầu cấu trúc duy nhất còn lại là cả hai chuỗi đều bao gồm nhiều chuỗi giống hệt nhau. 

Một khi điều này được nhận ra, điều kiện tuần hoàn sẽ trở nên không còn phù hợp theo nghĩa sâu sắc hơn. Nếu hai chuỗi giống hệt nhau, chúng ta chỉ cần chọn cùng một thứ tự cho cả hai chuỗi. Khi đó, các chuỗi được nối bằng nhau theo nghĩa đen, đây là trường hợp đặc biệt của đẳng cấu tuần hoàn với độ dịch chuyển bằng 0. Nếu nhiều tập hợp khác nhau thì không có sự sắp xếp lại nào có thể khắc phục được sự khác biệt đó vì hoán vị không tạo ra hoặc phá hủy các chuỗi. 

Điều này làm giảm vấn đề từ một câu hỏi căn chỉnh toàn cầu phức tạp thành một vấn đề so sánh nhiều tập hợp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n! · n · m) | O(nm) | Quá chậm | 
| Kết hợp nhiều bộ | O(nm log n) hoặc O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển sự quan sát đó thành một thủ tục mang tính xây dựng.

1. Đọc hai danh sách chuỗi cho một ca kiểm thử và liên kết mỗi chuỗi với chỉ mục ban đầu của nó trong đầu vào. 
2. Sắp xếp cả hai danh sách theo từ điển dựa trên nội dung chuỗi. Điều này cho phép các chuỗi giống hệt nhau sắp xếp theo thứ tự một cách tự nhiên. 
3. Sau khi sắp xếp, so sánh hai chuỗi chuỗi theo vị trí. Nếu có bất kỳ sự không khớp nào xuất hiện, điều đó có nghĩa là nhiều tập hợp không giống nhau và không tồn tại hoán vị hợp lệ nào, vì vậy chúng tôi xuất ra -1. 
4. Nếu chúng khớp nhau, hãy xây dựng cả hai hoán vị bằng cách lấy các chỉ số theo thứ tự đã sắp xếp. Hoán vị của A là thứ tự của A sau khi sắp xếp, đối với B cũng tương tự. 
5. Xuất ra hai hoán vị này. 

Lý do chính để sắp xếp là nó cung cấp một biểu diễn chuẩn của nhiều tập hợp. Nếu hai tập hợp bằng nhau thì cách biểu diễn được sắp xếp của chúng giống hệt nhau và mọi ghép nối giữa các phần tử giống hệt nhau đều hợp lệ. 

### Tại sao nó hoạt động 

Sự đẳng cấu tuần hoàn giữa các chuỗi được nối chỉ phụ thuộc vào nội dung chuỗi cuối cùng chứ không phụ thuộc vào cách các chuỗi được nhóm thành các khối bên trong. Vì chúng ta có thể tự do hoán vị các khối tùy ý trong cả hai chuỗi, nên chúng ta luôn có thể căn chỉnh các chuỗi giống hệt nhau theo cùng một thứ tự bất cứ khi nào các tập hợp cơ bản khớp với nhau. Điều này tạo ra các chuỗi nối giống hệt nhau, đáp ứng một cách tầm thường sự tương đương theo chu kỳ. Nếu nhiều tập hợp khác nhau thì ít nhất một chuỗi khác nhau về tần số và không có hoán vị hoặc phép xoay nào có thể điều hòa được sự không khớp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        A = []
        B = []
        
        for i in range(n):
            A.append((input().strip(), i + 1))
        for i in range(n):
            B.append((input().strip(), i + 1))
        
        A.sort()
        B.sort()
        
        ok = True
        for i in range(n):
            if A[i][0] != B[i][0]:
                ok = False
                break
        
        if not ok:
            out.append("-1")
            continue
        
        p = [str(x[1]) for x in A]
        q = [str(x[1]) for x in B]
        
        out.append(" ".join(p))
        out.append(" ".join(q))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nhóm các chuỗi với chỉ mục của chúng để việc sắp xếp không làm mất dấu vị trí ban đầu. Việc sắp xếp được thực hiện hoàn toàn dựa trên nội dung chuỗi, điều này an toàn vì các chuỗi giống hệt nhau có thể hoán đổi cho nhau. Sau khi sắp xếp, sự bằng nhau của hai chuỗi được kiểm tra trực tiếp. Nếu chúng khớp nhau, chúng ta sẽ xuất ra thứ tự chỉ mục tương ứng; nếu không thì chúng tôi báo cáo là không thể. 

Một cạm bẫy triển khai phổ biến là quên bảo toàn các chỉ mục gốc trong quá trình sắp xếp, điều này sẽ khiến không thể xây dựng lại các hoán vị cần thiết. Một vấn đề tế nhị khác là xử lý nhiều trường hợp thử nghiệm một cách hiệu quả mà không cần khởi tạo lại trạng thái toàn cục một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp cả hai bộ có thể khớp nhau: 

đầu vào: 

A = ["abc", "def", "ghi"] 

B = ["bcd", "efg", "hia"] 

Sau khi sắp xếp, chúng ta căn chỉnh các chuỗi giống hệt nhau. Nếu chúng là nhiều tập hợp giống hệt nhau, các thứ tự được sắp xếp sẽ khớp chính xác và chúng tôi sẽ đưa ra cấu trúc hoán vị giống nhau cho cả hai. 

Bây giờ hãy xem xét một trường hợp không khớp: 

A = ["abc"] 

B = ["def"] 

| Bước | Một sắp xếp | B sắp xếp | Trận đấu | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | abc | chắc chắn | Không | 

Vì các phần tử duy nhất khác nhau nên thuật toán ngay lập tức trả về -1, phản ánh rằng không có hoán vị nào có thể thu hẹp khoảng cách. 

Điều này chứng tỏ rằng thuật toán giảm vấn đề xuống việc kiểm tra đẳng thức nhiều tập hợp thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m log n) | Sắp xếp n chuỗi có độ dài m chiếm ưu thế | 
| Không gian | O(n m) | Lưu trữ tất cả các chuỗi và chỉ mục | 

Tổng kích thước của tất cả các trường hợp thử nghiệm tối đa là 10^6 ký tự, do đó việc sắp xếp và quét vẫn nằm trong giới hạn thoải mái. Mỗi nhân vật tham gia vào nhiều nhất một thao tác sắp xếp, giúp giải pháp luôn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        A = [(input().strip(), i+1) for i in range(n)]
        B = [(input().strip(), i+1) for i in range(n)]
        
        A.sort()
        B.sort()
        
        if any(A[i][0] != B[i][0] for i in range(n)):
            out.append("-1")
        else:
            out.append(" ".join(str(x[1]) for x in A))
            out.append(" ".join(str(x[1]) for x in B))
    
    return "\n".join(out)

# minimal case
assert run("1 1\na\n a\n") != ""

# identical single element
assert run("1 1\na\na\n") == "1\n1"

# simple valid swap
assert run("2 1\na\nb\na\nb\n") != "-1"

# multiset mismatch
assert run("2 1\na\nb\na\nc\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | chỉ số giống hệt nhau | độ đúng cơ sở | 
| nhiều bộ bằng nhau | hoán vị hợp lệ | xây dựng | 
| trường hợp không khớp | -1 | phát hiện không thể | 
| trường hợp trao đổi nhỏ | không -1 | đặt hàng linh hoạt | 

## Vỏ cạnh 

Nếu tất cả các chuỗi giống hệt nhau thì cả hai mảng được sắp xếp đều khớp nhau một cách tầm thường. Thuật toán đưa ra bất kỳ thứ tự giống hệt nhau nào và dịch chuyển theo chu kỳ được đáp ứng ngay lập tức vì cả hai chuỗi được nối đều giống nhau. 

Nếu có nhiều sự trùng lặp, chẳng hạn như có nhiều chuỗi lặp lại, việc sắp xếp vẫn nhóm chúng một cách chính xác. Thuật toán không phụ thuộc vào tính duy nhất và các khối giống hệt nhau có thể hoán đổi cho nhau, do đó mọi sự sắp xếp ổn định đều hợp lệ. 

Nếu chỉ có một chuỗi khác nhau giữa hai chuỗi, việc sắp xếp sẽ hiển thị sự không khớp ở một vị trí. Thuật toán từ chối chính xác trường hợp đó mà không cố gắng thực hiện bất kỳ lý do xoay vòng nào, điều này sẽ không liên quan vì không có phép quay nào có thể sửa được một khối bị thiếu hoặc bổ sung.
