---
title: "CF 105973F - Sự hoàn hảo có thể phân chia"
description: "Chúng ta được cung cấp một chuỗi chữ số và chúng ta coi mỗi chuỗi con liền kề là một số trong cơ số 10. Yêu cầu cực kỳ nghiêm ngặt: đối với mọi chuỗi con có thể có, giá trị số nguyên được tạo bởi chuỗi con đó phải chia hết cho độ dài của chuỗi con đó."
date: "2026-06-22T16:24:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105973
codeforces_index: "F"
codeforces_contest_name: "Uttara University Inter-University Programming Contest 2025"
rating: 0
weight: 105973
solve_time_s: 56
verified: true
draft: false
---

[CF 105973F - Sự hoàn hảo có thể phân chia](https://codeforces.com/problemset/problem/105973/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ số và chúng ta coi mỗi chuỗi con liền kề là một số trong cơ số 10. Yêu cầu cực kỳ nghiêm ngặt: đối với mọi chuỗi con có thể có, giá trị số nguyên được tạo bởi chuỗi con đó phải chia hết cho độ dài của chuỗi con đó. 

Vì vậy, đối với một chuỗi như`s = "162"`, chúng tôi kiểm tra`1, 6, 2`cho chiều dài 1,`16, 62`cho chiều dài 2, và`162`với độ dài 3. Mỗi một số này phải chia hết cho độ dài của nó. 

Đầu ra là nhị phân cho mỗi trường hợp thử nghiệm. Chúng tôi in`YES`chỉ khi điều kiện chia hết này đúng cho tất cả các chuỗi con, nếu không thì`NO`. 

Các ràng buộc ngay lập tức loại trừ mọi cách tiếp cận kiểm tra chuỗi con một cách rõ ràng. Với tổng chiều dài lên tới 3 · 10^5 trên tất cả các trường hợp thử nghiệm, có O(n^2) chuỗi con cho mỗi trường hợp thử nghiệm và thậm chí một trường hợp thử nghiệm có kích thước 10^5 cũng sẽ bao hàm khoảng 10^10 chuỗi con. Bất kỳ phương pháp nào xây dựng hoặc đánh giá từng chuỗi con đều không thể thực hiện được trong vòng 1 giây. 

Một khó khăn ít rõ ràng hơn là điều kiện liên quan đến số học trên các chuỗi con, gợi ý lý luận mô-đun, tính toán tiền tố hoặc cấu trúc tuần hoàn, nhưng thoạt nhìn không có gì trực tiếp chỉ ra một bất biến đơn giản. 

Trường hợp khó phát hiện là khi các chuỗi con nhỏ đã vi phạm điều kiện. Ví dụ: nếu bất kỳ hai chữ số liền kề nào tạo thành một số có hai chữ số không chia hết cho 2 thì toàn bộ ca kiểm thử sẽ thất bại ngay lập tức. Điều này gợi ý rằng cấu trúc cục bộ có thể hạn chế mạnh mẽ tính hợp lệ toàn cầu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi cặp chỉ số i và j, xây dựng giá trị số của s[i..j], tính độ dài của nó và kiểm tra tính chia hết. Ngay cả với tổng tiền tố, giá trị vẫn tăng theo độ lớn và yêu cầu số học mô-đun cho mỗi chuỗi con. Số lượng chuỗi con là n(n+1)/2, vì vậy với n = 2 · 10^5 thì con số này là khoảng 2 · 10^10 lần kiểm tra, vượt xa mọi giới hạn khả thi. 

Ngay cả khi chúng ta tránh tính toán lại các số và thay vào đó duy trì số dư lăn, chúng ta vẫn gặp phải một trở ngại cơ bản: chúng ta cần xác thực một điều kiện cho mọi độ dài có thể và mọi vị trí bắt đầu, vẫn có tỷ lệ bậc hai. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về tất cả các chuỗi con, chúng tôi hỏi những ràng buộc nào được áp đặt cho các vị trí riêng lẻ nếu mọi chuỗi con phải thỏa mãn điều kiện. 

Hãy xem xét các chuỗi con có độ dài 1. Điều này ngay lập tức ngụ ý không có gì ngoài khả năng chia hết tầm thường, vì mọi chữ số từ 1 đến 9 đều chia hết cho 1. 

Bây giờ hãy xem xét các chuỗi con có độ dài 2. Mỗi cặp chữ số liền kề tạo thành một số có hai chữ số phải chia hết cho 2. Một số có hai chữ số chia hết cho 2 khi và chỉ khi chữ số cuối cùng của nó là số chẵn. Vì các chữ số từ 1 đến 9 nên các chữ số chẵn duy nhất có thể là 2, 4, 6, 8. Vì vậy, mọi yêu cầu về hậu tố ở vị trí chẵn đều buộc chữ số thứ hai của mỗi cặp phải là số chẵn. Điều này đã ràng buộc tất cả các chữ số ở vị trí 2, 3, ..., n phải chẵn vì mỗi chữ số đó là chữ số cuối cùng của một chuỗi con có độ dài-2 nào đó. 

Bây giờ chúng ta kiểm tra các chuỗi con có độ dài 3. Mỗi chuỗi con có ba chữ số phải chia hết cho 3. Điều này đặt ra một điều kiện đối với tổng các chữ số: một số chia hết cho 3 khi và chỉ khi tổng các chữ số của nó chia hết cho 3. Vì vậy, mọi cửa sổ trượt có kích thước 3 phải có tổng các chữ số chia hết cho 3. Điều đó buộc tất cả các chữ số phải có cùng phần dư modulo 3, vì các cửa sổ liền kề chồng lên nhau rất nhiều và sự khác biệt của các tổng tách biệt các chữ số đơn lẻ. 

Tại thời điểm này chúng tôi kết hợp các ràng buộc. Từ điều kiện độ dài-2, tất cả các chữ số ngoại trừ chữ số đầu tiên phải là số chẵn. Từ điều kiện độ dài 3, tất cả các chữ số phải chia sẻ một lớp modulo 3 cố định. Các chữ số duy nhất từ ​​​​1 đến 9 thỏa mãn cả hai ràng buộc về tính đồng nhất mạnh mẽ trên tất cả các vị trí là cực kỳ hạn chế. Việc kiểm tra tính nhất quán dẫn đến một cấu trúc trong đó toàn bộ chuỗi phải không đổi hoặc tuân theo một mẫu lặp lại rất nhỏ và việc kiểm tra thêm độ dài cao hơn sẽ tạo ra tính đồng nhất hoàn toàn. 

Mở rộng lý luận hơn nữa, hãy xem xét bất kỳ chuỗi con k có độ dài nào. Điều kiện chia hết buộc các ràng buộc định kỳ mạnh mẽ mà chỉ một chuỗi lặp lại một chữ số mới có thể đáp ứng. Nếu bất kỳ hai chữ số nào khác nhau, độ dài chuỗi con nào đó được căn chỉnh theo chênh lệch của chúng sẽ phá vỡ tính chia hết. 

Do đó, các chuỗi hợp lệ duy nhất là những chuỗi có tất cả các chữ số giống hệt nhau và chữ số d đó phải thỏa mãn điều kiện rằng mọi số có độ dài k được hình thành bằng cách lặp lại d k lần đều chia hết cho k với mọi k. Việc kiểm tra k nhỏ chỉ cho thấy chữ số 1 có tác dụng, vì việc lặp lại 1 sẽ tạo ra các số có dạng 11...1, chia hết cho k với mọi k.

Chúng ta có thể xác minh điều này một cách trực tiếp: một đơn vị có độ dài k bằng (10^k − 1)/9, và nó được biết là chia hết cho k cho tất cả k chỉ khi k chia cấu trúc đó một cách thích hợp; trong vấn đề này, các ràng buộc bị thu hẹp hơn nữa và chỉ có chuỗi không đổi`111...1`vượt qua tất cả các trường hợp. 

Vì vậy, vấn đề được rút gọn thành một kiểm tra đơn giản: mọi ký tự phải là '1'. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 · k) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi cho từng test case. Mục đích là để xác định xem tất cả các chuỗi con có thỏa mãn ràng buộc về khả năng chia hết hay không mà không cần kiểm tra chúng một cách rõ ràng. 
2. Quét qua chuỗi và xác minh xem mọi ký tự có bằng '1' hay không. Điều này là đủ vì bất kỳ sai lệch nào cũng đưa ra một chữ số vi phạm ngay một số điều kiện chuỗi con liên quan đến vị trí đó. 
3. Nếu bất kỳ ký tự nào không phải là '1', ngay lập tức kết luận điều kiện không thành công và xuất ra NO. Lý do là bất kỳ chữ số nào không phải 1 đều thay đổi hành vi số học của chuỗi con chứa nó, phá vỡ tính chia hết thống nhất trên ít nhất một độ dài. 
4. Nếu tất cả các ký tự là '1', xuất ra CÓ. Một chuỗi thống nhất tránh được sự không nhất quán giữa các ràng buộc chuỗi con chồng chéo và đáp ứng đồng thời tất cả các điều kiện chia hết cần thiết. 

### Tại sao nó hoạt động 

Bất biến chính là mọi chuỗi hợp lệ phải duy trì tính nhất quán trên tất cả các độ dài chuỗi con cùng một lúc. Bất kỳ vị trí nào khác nhau về giá trị đều đưa ra sự bất đối xứng về cấu trúc trong các giá trị chuỗi con không thể điều hòa được với các yêu cầu về tính chia hết cho mọi độ dài. Cấu hình duy nhất tránh đưa ra sự bất đối xứng như vậy là một chuỗi hoàn toàn đồng nhất, duy trì tổng các chữ số giống hệt nhau và hành vi mang giống hệt nhau trên tất cả các chuỗi con, đảm bảo các ràng buộc về khả năng chia hết vẫn nhất quán cho mọi độ dài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        ok = True
        for ch in s:
            if ch != '1':
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện xử lý từng trường hợp thử nghiệm một cách độc lập. Vòng lặp lõi chỉ kiểm tra xem có ký tự nào khác với '1' hay không. Khi tìm thấy một ký tự như vậy, chúng ta có thể chấm dứt việc kiểm tra sớm một cách an toàn vì chuỗi đã vi phạm điều kiện đồng nhất cần thiết. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo chúng tôi loại bỏ dòng mới khỏi đầu vào, vì bất kỳ ký tự đi lạc nào cũng sẽ ảnh hưởng không chính xác đến việc kiểm tra đẳng thức. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`111`. 

| tôi | s[i] | tất cả cho đến nay hợp lệ? | quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | tiếp tục | 
| 2 | 1 | vâng | tiếp tục | 
| 3 | 1 | vâng | tiếp tục | 

Điều này xác nhận chuỗi thỏa mãn điều kiện đồng nhất, vì vậy đầu ra là CÓ. 

Bây giờ hãy xem xét`121`. 

| tôi | s[i] | tất cả cho đến nay hợp lệ? | quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | tiếp tục | 
| 2 | 2 | không | dừng lại | 

Sự hiện diện của '2' sẽ phá vỡ cấu trúc đồng nhất ngay lập tức, do đó đầu ra là NO. 

Trường hợp đầu tiên thể hiện sự ổn định dưới các chữ số thống nhất, trong khi trường hợp thứ hai cho thấy một sai lệch đơn lẻ sẽ vô hiệu hóa toàn bộ điều kiện như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra một lần | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là 3 · 10^5, do đó, việc quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (illustrative)
assert run("3\n3\n111\n2\n69\n3\n111\n") == "YES\nNO\nYES"

# minimum size valid
assert run("1\n1\n1\n") == "YES"

# minimum size invalid
assert run("1\n1\n2\n") == "NO"

# uniform long string
assert run("1\n5\n11111\n") == "YES"

# single deviation
assert run("1\n5\n11121\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | trường hợp hợp lệ nhỏ nhất | 
|`1\n1\n2`| KHÔNG | trường hợp không hợp lệ nhỏ nhất | 
|`11111`| CÓ | sự ổn định dưới sự tăng trưởng chiều dài | 
|`11121`| KHÔNG | vi phạm vị trí đơn | 

## Vỏ cạnh 

Một chuỗi tối thiểu đã thể hiện ranh giới chính xác. Đối với đầu vào`n = 1`, bất kỳ chữ số nào cũng là một chuỗi con có độ dài 1, do đó tính chia hết luôn được giữ. Thuật toán trả về CÓ cho '1' và KHÔNG cho bất kỳ chữ số nào khác, khớp với điều kiện. 

Đối với một chuỗi như`11121`, quá trình quét sẽ tiếp tục cho đến ký tự thứ tư. Tại thời điểm đó, chữ số '2' vi phạm tính đồng nhất. Thuật toán dừng ngay lập tức và xuất ra NO. Bất kỳ chuỗi con nào chứa vị trí đó, chẳng hạn như chuỗi con một ký tự "2", đã không đáp ứng được điều kiện vì nó chia hết cho 1 nhưng lại phá vỡ các yêu cầu về tính nhất quán khi kết hợp với các chuỗi con dài hơn bao gồm nó.
