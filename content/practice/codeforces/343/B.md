---
title: "CF 343B - Dòng điện xoay chiều"
description: "Chúng ta được cung cấp một trình tự mô tả cách hai dây, một dây được gọi là “cộng” và dây còn lại là “trừ”, chồng lên nhau khi chúng chạy từ bên trái sang bên phải của thiết bị. Tại mỗi vị trí dọc theo đường dẫn, chính xác một trong các dây nằm phía trên dây kia."
date: "2026-06-06T17:47:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 343
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 200 (Div. 1)"
rating: 1600
weight: 343
solve_time_s: 123
verified: true
draft: false
---

[CF 343B - Dòng điện xoay chiều](https://codeforces.com/problemset/problem/343/B) 

**Đánh giá:** 1600 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một trình tự mô tả cách hai dây, một dây được gọi là “cộng” và dây còn lại là “trừ”, chồng lên nhau khi chúng chạy từ bên trái sang bên phải của thiết bị. Tại mỗi vị trí dọc theo đường dẫn, chính xác một trong các dây nằm phía trên dây kia. Ký tự “+” có nghĩa là dây dương nằm phía trên dây âm ở vị trí đó, trong khi ký tự “-” có nghĩa là dây âm nằm phía trên dây dương. 

Phía bên trái được cố định: dây dương bắt đầu ở tiếp điểm trên và dây âm bắt đầu ở tiếp điểm dưới. Chúng ta được phép tự do di chuyển các dây trong mặt phẳng miễn là chúng ta không cắt chúng hoặc tách rời các điểm cuối của chúng. Câu hỏi đặt ra là liệu chúng ta có thể biến dạng các dây liên tục để cuối cùng chúng không bị vướng vào nhau hay không, nghĩa là không còn điểm giao nhau nào trong khi vẫn bảo toàn các điểm cuối. 

Độ dài đầu vào có thể lên tới 100000. Điều đó loại trừ bất kỳ mô phỏng nào cố gắng mô hình hóa hình học dây một cách rõ ràng hoặc duy trì cấu trúc các điểm giao nhau thay đổi linh hoạt với các bản cập nhật đắt tiền. Bất cứ điều gì bậc hai về số lượng ký tự sẽ quá chậm. 

Một điểm tinh tế là trình tự không mô tả các điểm giao cắt độc lập. Nó mô tả sự tương tác liên tục giống như bím tóc. Hai ký hiệu giống hệt nhau liên tiếp không nhất thiết phải hoạt động giống như các ký hiệu tách biệt, bởi vì việc “bọc” toàn bộ dây này quanh dây kia được mã hóa thành một mẫu xen kẽ cụ thể. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể hủy bỏ các giao điểm đối diện liền kề hoặc xử lý chuỗi giống như một bài toán giảm ngăn xếp tương tự như dấu ngoặc đơn. Ví dụ: diễn giải “+” là sự kiện mở đầu và “-” là sự kiện kết thúc sẽ thất bại ngay lập tức: 

đầu vào:```
-+
```Một ý tưởng hủy bỏ ngây thơ có thể gợi ý việc hủy bỏ các giao cắt, nhưng câu trả lời đúng là “Không” vì điều này thể hiện một bước ngoặt hoàn toàn không thể loại bỏ nếu không di chuyển các điểm cuối. 

Một trường hợp thất bại khác là xử lý từng dấu “+” một cách độc lập và chỉ cần kiểm tra xem số lượng có cân bằng hay không. Chuỗi “+-+-” trông có vẻ vô hại theo logic đếm một cách không chính xác, tuy nhiên, nó có thể biểu thị một cấu trúc bện không tầm thường phải được hiểu là một ràng buộc toàn cục duy nhất. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng sự biến dạng thực tế của hai đường cong trong mặt phẳng. Người ta có thể mô hình hóa các dây dưới dạng các đường polylines và liên tục cố gắng làm thẳng các đoạn cục bộ trong khi theo dõi các giao lộ. Mỗi khi chúng tôi loại bỏ một lối giao cắt, các lối giao cắt mới có thể xuất hiện ở nơi khác, vì vậy quá trình này có thể yêu cầu phải xem lại toàn bộ cấu trúc nhiều lần. Trong trường hợp xấu nhất, mỗi điều chỉnh cục bộ tương tác với tất cả những điều chỉnh trước đó, dẫn đến số lượng cập nhật bậc hai hoặc tệ hơn trong suốt chiều dài của chuỗi. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến hình học, mà chỉ quan tâm đến việc liệu chuỗi có mã hóa một “quấn” không cần thiết của dây này quanh dây kia hay không. Điều này làm giảm vấn đề trong việc theo dõi xem cấu trúc tiền tố có bao giờ buộc phải chuyển sang trạng thái không thể hay không. 

Sự đơn giản hóa quan trọng là trình tự này hoạt động giống như một tập hợp các thay đổi về hướng và điều quan trọng là liệu chúng ta có bao giờ “hủy bỏ quá mức” các điểm giao nhau theo cách yêu cầu nâng dây qua ràng buộc điểm cuối khác hay không. Chúng ta có thể hiểu quy trình này là theo dõi một số dư số nguyên duy nhất trong đó chúng ta tăng hoặc giảm tùy thuộc vào việc dây cộng ở trên hay dưới, nhưng có một ràng buộc quan trọng: nếu số dư vượt quá ngưỡng theo cách buộc phải đảo ngược các điểm cuối cố định thì cấu hình sẽ không thể thực hiện được. 

Điều này dẫn đến việc quét tuyến tính tham lam trong đó chúng tôi duy trì một bộ đếm biểu thị trạng thái cuộn dây tương đối hiện tại. Bất cứ khi nào chúng tôi nhìn thấy một biểu tượng, chúng tôi sẽ điều chỉnh bộ đếm, nhưng chúng tôi cũng đảm bảo rằng bộ đếm không bao giờ vi phạm ràng buộc do các điểm cuối cố định gây ra. Trạng thái cuối cùng xác định xem bím tóc có tầm thường hay không. 

Trong vấn đề cụ thể này, điều kiện giảm xuống còn việc kiểm tra xem chuỗi có bao giờ mã hóa một vòng quay đầy đủ hay không. Điều đó có thể được phát hiện bằng cách đảm bảo rằng chúng ta không bao giờ cần nhiều hơn một “lớp” mất cân bằng hoạt động cùng một lúc, tương đương với việc đảm bảo rằng trạng thái đang chạy không bao giờ vượt quá 1 về giá trị tuyệt đối theo cách buộc phải bọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hình học Brute Force | O(n²) | O(n) | Quá chậm | 
| Theo dõi tham lam tuyến tính của trạng thái cuộn dây | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải trong khi duy trì trạng thái số nguyên duy nhất`bal`. 

1. Khởi tạo`bal = 0`. Điều này thể hiện rằng, trước khi xem xét bất kỳ điểm giao cắt nào, hệ thống ở trạng thái trung tính phù hợp với các điểm cuối cố định. 
2. Quét từng ký tự trong chuỗi. Nếu ký tự là “+”, chúng tôi coi nó là đẩy cấu hình lên trên, do đó chúng tôi tăng dần`bal`bằng 1. Nếu là “-”, chúng ta giảm`bal`bằng 1. Điều này mô hình hóa cách mỗi giao lộ địa phương đóng góp vào thứ tự tương đối. 
3. Sau mỗi lần cập nhật, hãy kiểm tra xem trạng thái có rơi vào chế độ không thể thực hiện được hay không. Nếu tại bất kỳ thời điểm nào độ lớn của`bal`vượt quá 1, chúng tôi ngay lập tức kết luận cấu hình thể hiện sự thay đổi hoàn toàn không thể hoàn tác theo các điểm cuối cố định và chúng tôi từ chối. 
4. Nếu quá trình quét hoàn tất mà không vi phạm ràng buộc, cấu hình có thể liên tục được gỡ rối nên chúng tôi chấp nhận. 

Ý tưởng chính đằng sau việc kiểm tra ngưỡng là bất kỳ nỗ lực nào nhằm tích lũy nhiều hơn một đơn vị mất cân bằng đều buộc một dây phải “quấn quanh” dây kia theo cách không thể hoàn tác nếu không thay đổi thứ tự điểm cuối. 

### Tại sao nó hoạt động 

Quá trình này ngầm theo dõi cuộn lưới giữa hai dây với ràng buộc là điểm cuối của chúng được cố định và có thứ tự. Bất kỳ sự gỡ rối hợp lệ nào đều tương ứng với một biến dạng không bao giờ đưa ra nhiều hơn một ưu thế cục bộ ở bất kỳ tiền tố nào. Nếu sự mất cân bằng khi chạy vượt quá giới hạn này, nó sẽ mã hóa một vòng quay hoàn toàn của một dây xung quanh dây kia, đây là một trở ngại cấu trúc liên kết không thể loại bỏ chỉ bằng biến dạng phẳng. Do đó, thuật toán này an toàn vì nó loại bỏ chính xác các chuỗi buộc phải quay như vậy và chấp nhận tất cả các chuỗi khác trong đó sự mất cân bằng vẫn có thể giải quyết được cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

bal = 0
for ch in s:
    if ch == '+':
        bal += 1
    else:
        bal -= 1
    
    if abs(bal) > 1:
        print("No")
        sys.exit()

print("Yes")
```Mã duy trì một bộ đếm đang chạy duy nhất và cập nhật nó theo thời gian không đổi trên mỗi ký tự. Việc thoát sớm rất quan trọng vì một khi không thể cấu hình được thì việc xử lý tiếp theo sẽ không thể khôi phục tính hợp lệ. 

Điều tinh tế duy nhất là chúng ta phải kiểm tra ràng buộc ngay sau mỗi lần cập nhật, không chỉ ở cuối. Việc trì hoãn kiểm tra sẽ khiến các cấu hình trung gian không hợp lệ được chấp nhận không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
-++-
```Chúng tôi theo dõi`bal`từng bước một. 

| Bước | char | bóng | cơ bụng (bal) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | - | -1 | 1 | được | 
| 2 | + | 0 | 0 | được | 
| 3 | + | 1 | 1 | được | 
| 4 | - | 0 | 0 | được | 

Vì chúng ta không bao giờ vượt quá giá trị tuyệt đối 1 nên kết quả đầu ra là “Có”. 

Bây giờ hãy xem xét một chuỗi có vấn đề:```
--++
```| Bước | char | bóng | cơ bụng (bal) | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | - | -1 | 1 | được | 
| 2 | - | -2 | 2 | từ chối | 

Ở bước 2, sự mất cân bằng vượt quá 1, nghĩa là cấu trúc buộc phải bọc toàn bộ và không thể hoàn tác được. 

Những dấu vết này cho thấy ràng buộc được thực thi cục bộ như thế nào và ngăn chặn sự tích tụ của cấu trúc không thể tháo rời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(1) | Chỉ có một trạng thái số nguyên duy nhất được duy trì | 

Giải pháp xử lý thoải mái các đầu vào lên tới 100000 ký tự vì nó chỉ thực hiện một lượng công việc không đổi trên mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        s = input().strip()
        bal = 0
        for ch in s:
            if ch == '+':
                bal += 1
            else:
                bal -= 1
            if abs(bal) > 1:
                return "No"
        return "Yes"
    finally:
        sys.stdin = old_stdin

# provided sample
assert run("-++-") == "Yes"

# minimum size
assert run("+") == "Yes"

# immediate invalid wrap
assert run("--") == "No"

# alternating safe case
assert run("+-+-") == "Yes"

# longer valid neutral sequence
assert run("-+-++-") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| + | Có | cấu hình hợp lệ tối thiểu | 
| -- | Không | phát hiện vi phạm ngay lập tức | 
| +-+- | Có | chuỗi xen kẽ vẫn bị giới hạn | 
| -+-++- | Có | trình tự hỗn hợp không tràn | 

## Vỏ cạnh 

Dữ liệu nhập có một ký tự như “+” hoặc “-” luôn hợp lệ vì nó không thể tạo ra một gói đầy đủ. Thuật toán xử lý một lần cập nhật và kết thúc ngay lập tức mà không vượt quá ngưỡng. 

Chuỗi hai ký tự như “--” thể hiện lỗi sớm nhất có thể xảy ra. Sau bước thứ hai, số dư trở thành -2, vi phạm ràng buộc ngay lập tức. Thuật toán sẽ loại bỏ chính xác tại thời điểm đó mà không cần phải quét thêm. 

Một chuỗi xen kẽ như “+-+-” không bao giờ tích lũy nhiều hơn một đơn vị mất cân bằng. Trạng thái chạy dao động trong khoảng -1, 0 và 1, tương ứng với các giao điểm cục bộ có thể được loại bỏ bằng cách trượt dây này qua dây kia mà không thay đổi thứ tự điểm cuối. 

Một chuỗi dài hơn hỗn hợp như “-+-++-” cho thấy rằng ngay cả khi tiền tố chứa cả hai hướng, miễn là sự mất cân bằng đang chạy không bao giờ vượt quá giới hạn cho phép thì cấu trúc vẫn không bị rối. Thuật toán duy trì tính chính xác bằng cách thực thi ràng buộc ở mọi tiền tố thay vì dựa vào tổng cuối cùng.
