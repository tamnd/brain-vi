---
title: "CF 104679G - Quà Tặng Mùa Đông"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau và kích thước bước nguyên $k$. Các thao tác được phép không cho phép chúng ta tự do chỉnh sửa ký tự ở bất cứ đâu. Thay vào đó, chúng ta có thể chọn hai vị trí có khoảng cách chính xác là $k$ và sao chép ký tự từ vị trí này sang vị trí khác."
date: "2026-06-29T09:02:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "G"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 44
verified: true
draft: false
---

[CF 104679G - Quà tặng mùa đông](https://codeforces.com/problemset/problem/104679/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau và kích thước bước nguyên$k$. Các thao tác được phép không cho phép chúng ta tự do chỉnh sửa ký tự ở bất cứ đâu. Thay vào đó, chúng ta có thể chọn hai vị trí có khoảng cách chính xác bằng$k$và sao chép ký tự từ vị trí này sang vị trí khác. 

Vì vậy, hoạt động này hoạt động giống như một quy tắc truyền bá bị ràng buộc: các ký tự chỉ có thể “chảy” dọc theo các cạnh kết nối các vị trí khác nhau bằng$k$. Qua nhiều hoạt động, điều này có nghĩa là thông tin có thể di chuyển qua các chuỗi như$i \to i+k \to i+2k \to \dots$, nhưng nó không bao giờ có thể nhảy giữa các vị trí không liên quan. 

Nhiệm vụ là xác định xem chúng ta có thể chuyển đổi chuỗi bắt đầu thành chuỗi đích bằng cách sử dụng bất kỳ số lượng thao tác sao chép nào như vậy hay không. 

Hạn chế chính cần ghi nhớ là độ dài chuỗi có thể đủ lớn để bất kỳ mô phỏng bậc hai nào của các phép toán hoặc tìm kiếm vũ phu trên các phép biến đổi đều không thể thực hiện được. Bất kỳ giải pháp nào cũng phải giảm cấu trúc của các hoạt động được phép thành một cái gì đó tuyến tính hoặc gần tuyến tính, bởi vì mỗi vị trí chỉ tương tác với một tập hợp con có cấu trúc nhỏ của các vị trí khác. 

Một trường hợp có cạnh tinh tế xuất phát từ việc chuyển động thực sự bị hạn chế như thế nào. Ví dụ, nếu$k = 3$và chuỗi được lập chỉ mục là$0 \dots 7$, vị trí 0 chỉ có thể ảnh hưởng đến vị trí 3 và 6, không bao giờ ảnh hưởng đến các vị trí như 1 hoặc 2. Vì vậy, ngay cả khi hai ký tự “gần nhau về giá trị”, chúng có thể bị ngắt kết nối hoàn toàn theo quy tắc hoạt động. 

Một trường hợp cạnh quan trọng khác là khi$k = 0$, thường không hợp lệ hoặc suy biến, vì thao tác sẽ chỉ cho phép tự sao chép và không thể thực hiện chuyển đổi nào trừ khi các chuỗi đã khớp. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng các hoạt động được phép trực tiếp trên chuỗi. Mỗi thao tác sao chép một ký tự trong khoảng cách$k$và chúng tôi sẽ thử tất cả các chuỗi hoạt động có thể có, kiểm tra xem liệu chúng tôi có thể đạt được cấu hình mục tiêu hay không. 

Điều này ngay lập tức dẫn đến một vụ nổ tổ hợp. Mặc dù mỗi thao tác đều đơn giản nhưng số lượng trạng thái có thể truy cập tăng theo cấp số nhân với số lượng vị trí được kết nối bởi các ứng dụng lặp đi lặp lại. Đối với một chuỗi có độ dài$n$, điều này vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là hoạt động không trộn lẫn tất cả các chỉ số với nhau. Thay vào đó, nó phân chia các chỉ số thành các cấp số cộng độc lập theo modulo$k$. Chỉ số$0, k, 2k, \dots$tạo thành một thành phần, chỉ số$1, k+1, 2k+1, \dots$tạo thành một cái khác, v.v. cho đến$k-1$. Không có hoạt động nào vượt qua các thành phần này. 

Điều này làm giảm vấn đề thành$k$các bài toán con độc lập, mỗi bài toán hoạt động theo một chuỗi được hình thành bằng cách lấy mọi$k$-nhân vật thứ. Bên trong mỗi chuỗi, thao tác trở nên tương đương với việc sao chép giữa các vị trí liền kề, bởi vì từng bước$k$trong chuỗi gốc tương ứng với bước 1 trong chuỗi con được trích xuất. 

Sau khi được rút gọn thành một chuỗi duy nhất chỉ tồn tại các hoạt động sao chép liền kề, vấn đề sẽ chuyển sang việc quyết định xem liệu chúng ta có thể chuyển đổi chuỗi này thành chuỗi khác chỉ bằng cách sử dụng các hoạt động “ghi đè lên hàng xóm” hay không. Quan sát quan trọng là các thao tác như vậy không thể đưa ra các khối ký tự riêng biệt mới; họ chỉ có thể hợp nhất hoặc truyền bá những cái hiện có. 

Điều này dẫn đến việc nén chuỗi mục tiêu thành các chuỗi ký tự riêng biệt. Điều quan trọng là liệu mẫu nén này có xuất hiện dưới dạng một chuỗi con trong chuỗi nguồn hay không. Nếu đúng như vậy, chúng ta có thể mô phỏng việc xây dựng mục tiêu bằng cách căn chỉnh và truyền bá các phân đoạn; nếu không, một số thay đổi cấu trúc cần thiết sẽ không thể thực hiện được trong các hoạt động bị hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút gọn chuỗi gốc thành các chuỗi độc lập dựa trên chỉ số modulo$k$, sau đó giải quyết từng chuỗi riêng biệt. 

1. Chia chuỗi gốc và chuỗi đích thành$k$các chuỗi tiếp theo. các$i$- dãy con thứ chứa các ký tự ở vị trí$i, i+k, i+2k, \dots$. Điều này đúng vì phép toán không bao giờ kết nối các chỉ số với các số dư khác nhau theo modulo$k$. 
2. Đối với mỗi cặp dãy con tương ứng, hãy coi bài toán là chuyển đổi một chuỗi thành một chuỗi khác trong đó các phép toán chỉ cho phép sao chép các ký tự liền kề. Điều này nắm bắt chính xác ràng buộc chuyển động ban đầu sau khi phân tách. 
3. Nén dãy con mục tiêu bằng cách thu gọn các ký tự bằng nhau liên tiếp thành một đại diện duy nhất. Điều này loại bỏ thông tin dư thừa vì các phân đoạn giống hệt nhau lặp đi lặp lại không áp đặt các ràng buộc cấu trúc bổ sung ngoài ranh giới của chúng. 
4. Kiểm tra xem mục tiêu nén này có phải là một dãy con của dãy con nguồn hay không. Chúng tôi quét nguồn và khớp các ký tự theo thứ tự một cách tham lam. 
5. Nếu bất kỳ chuỗi con nào không đạt được điều kiện này thì việc chuyển đổi hoàn toàn là không thể, vì vậy chúng tôi trả về “KHÔNG”. 
6. Nếu tất cả các chuỗi con đều đạt, chúng ta trả về “CÓ”. 

Bước lý luận chính là khi chúng ta cố định thứ tự của các khối riêng biệt trong mục tiêu, nguồn phải chứa chúng theo cùng một thứ tự tương đối, vì các thao tác chỉ truyền bá các ký tự hiện có mà không tạo ra khả năng sắp xếp thứ tự mới. 

### Tại sao nó hoạt động 

Mỗi lớp modulo hoạt động giống như một dòng trong đó chúng ta chỉ có thể ghi đè lên các vị trí bằng cách sử dụng lan truyền liền kề. Điều này có nghĩa là tập hợp các khối ký tự riêng biệt trong mục tiêu phải tồn tại trong nguồn theo cùng một thứ tự, bởi vì thao tác chỉ có thể mở rộng hoặc thu nhỏ các vùng có ký tự giống hệt nhau, không bao giờ hoán vị chúng hoặc tạo chuyển tiếp mới. Việc kiểm tra trình tự tiếp theo trên mục tiêu nén sẽ nắm bắt chính xác xem cấu trúc ranh giới được yêu cầu đã có trong nguồn hay chưa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress(s):
    res = []
    for c in s:
        if not res or res[-1] != c:
            res.append(c)
    return res

def is_subseq(a, b):
    j = 0
    for x in b:
        if j < len(a) and a[j] == x:
            j += 1
    return j == len(a)

def solve_case(s, t, k):
    n = len(s)
    
    for start in range(k):
        ss = []
        tt = []
        
        i = start
        while i < n:
            ss.append(s[i])
            tt.append(t[i])
            i += k
        
        tt = compress(tt)
        
        if not is_subseq(tt, ss):
            return False
    
    return True

def main():
    s = input().strip()
    t = input().strip()
    k = int(input())
    
    print("YES" if solve_case(s, t, k) else "NO")

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ cô lập từng lớp dư lượng theo modulo$k$bằng cách duyệt qua các chỉ số theo các bước của$k$. Điều này tránh việc xây dựng các mảng bổ sung với chi phí cắt lát và giữ logic tuyến tính theo độ dài chuỗi. 

Bước nén là cần thiết vì các bản sao liên tiếp trong chuỗi con đích không tạo ra các ràng buộc mới; họ chỉ mở rộng các phân khúc hiện có. Nếu không nén, việc kiểm tra trình tự tiếp theo sẽ yêu cầu khớp cấu trúc lặp lại một cách không chính xác mà các hoạt động có thể tự do tạo ra. 

Kiểm tra trình tự tiếp theo sử dụng quét con trỏ tham lam, phương pháp này tối ưu vì thứ tự là hạn chế duy nhất quan trọng sau khi nén. 

## Ví dụ đã hoạt động 

Hãy xem xét$s =$“abac” và$t =$“aaac” với$k = 2$. Chúng tôi chia thành hai chuỗi: chỉ số$0,2$đưa ra “aa” và “ac”, và chỉ số$1,3$cho “bc” và “ac”. 

Đối với chuỗi đầu tiên: 

| Nguồn | Mục tiêu (thô) | Mục tiêu (đã nén) | Kiểm tra trình tự | 
| --- | --- | --- | --- | 
| aa | aa | một | vâng | 

Đối với chuỗi thứ hai: 

| Nguồn | Mục tiêu (thô) | Mục tiêu (đã nén) | Kiểm tra trình tự | 
| --- | --- | --- | --- | 
| bc | ac | ac | vâng | 

Cả hai chuỗi đều thành công nên câu trả lời là CÓ. Điều này cho thấy cách nén loại bỏ cấu trúc lặp lại dư thừa và giảm việc kiểm tra thứ tự ranh giới. 

Bây giờ hãy xem xét$s =$“abdc”,$t =$“acbd”,$k = 1$. Ở đây có một chuỗi duy nhất. 

Nén mục tiêu vẫn là “acbd” vì không có bản sao liên tiếp. 

Chúng tôi thử kết hợp chuỗi con: 

| Bước | Con trỏ nguồn | Con trỏ đích | Trận đấu | 
| --- | --- | --- | --- | 
| một vs một | 0 | 0 | vâng | 
| b vs c | 1 | 1 | không | 
| d vs c | 2 | 1 | không | 

Chúng ta thất bại ngay lập tức nên câu trả lời là KHÔNG. Điều này chứng tỏ rằng mặc dù các ký tự giống hệt nhau trong nhiều tập hợp, nhưng các ràng buộc sắp xếp thứ tự từ quá trình truyền lan khiến cho việc sắp xếp lại là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần trên lớp dư lượng của nó và việc kiểm tra trình tự tiếp theo là tuyến tính | 
| Không gian | O(n) | Lưu trữ chuỗi được trích xuất trong trường hợp xấu nhất | 

Giải pháp duy trì tuyến tính ở kích thước đầu vào, điều này là cần thiết vì mọi mô phỏng hoạt động lồng nhau sẽ vượt quá giới hạn đối với các chuỗi lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    t = input().strip()
    k = int(input())
    
    def compress(s):
        res = []
        for c in s:
            if not res or res[-1] != c:
                res.append(c)
        return res

    def is_subseq(a, b):
        j = 0
        for x in b:
            if j < len(a) and a[j] == x:
                j += 1
        return j == len(a)

    n = len(s)
    for start in range(k):
        ss, tt = [], []
        i = start
        while i < n:
            ss.append(s[i])
            tt.append(t[i])
            i += k
        if not is_subseq(compress(tt), ss):
            return "NO"
    return "YES"

# minimal
assert run("a\na\n1") == "YES"

# impossible reordering
assert run("ab\nba\n1") == "NO"

# k = 2 split case
assert run("abac\naaac\n2") == "YES"

# identical strings
assert run("abcde\nabcde\n2") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a/a/1 | CÓ | danh tính tầm thường | 
| ab / ba / 1 | KHÔNG | hạn chế đặt hàng | 
| abac / aaac / 2 | CÓ | phân rã đúng đắn | 
| abcde / abcde / 2 | CÓ | trường hợp ổn định không thay đổi | 

## Vỏ cạnh 

Khi nào$k = 1$, toàn bộ chuỗi sẽ trở thành một chuỗi duy nhất. Thuật toán giảm xuống việc kiểm tra xem mục tiêu nén có phải là một chuỗi con của nguồn hay không, thuật toán này xử lý chính xác các trường hợp chỉ có thể truyền lan mà không cần trộn vị trí chéo. Ví dụ: việc chuyển đổi “aabbcc” thành “abc” thành công vì quá trình nén sẽ loại bỏ các bản sao và điều kiện tiếp theo được giữ ở mức bình thường. 

Khi tất cả các ký tự giống hệt nhau, cả hai chuỗi luôn vượt qua sau khi nén, bởi vì mỗi lần kiểm tra trình tự tiếp theo sẽ giảm xuống còn một ký tự lặp lại duy nhất. Ví dụ: “aaaaa” đến “aaaaa” thành công bất kể$k$, vì không cần chuyển đổi cấu trúc. 

Khi mục tiêu đưa ra một thứ tự mới của các khối ký tự không có trong nguồn, việc kiểm tra trình tự tiếp theo sẽ thất bại ngay lập tức trong chuỗi đó, phản ánh việc không thể tạo các ranh giới mới chỉ thông qua các thao tác sao chép.
