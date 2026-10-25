---
title: "CF 106068A - Giá đỡ đúng"
description: "Chúng ta được cung cấp một chuỗi chỉ gồm dấu ngoặc đơn mở và đóng. Thao tác duy nhất được phép là lấy dấu ngoặc đơn đóng từ một vị trí nào đó và chèn lại nó vào bất kỳ vị trí nào bên trái. Dấu ngoặc đơn mở được cố định tại chỗ."
date: "2026-06-21T09:21:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "A"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 49
verified: true
draft: false
---

[CF 106068A - Dấu ngoặc đúng](https://codeforces.com/problemset/problem/106068/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ gồm dấu ngoặc đơn mở và đóng. Thao tác duy nhất được phép là lấy dấu ngoặc đơn đóng từ một vị trí nào đó và chèn lại nó vào bất kỳ vị trí nào bên trái. Dấu ngoặc đơn mở được cố định tại chỗ. 

Mục đích là để xác định xem liệu sau bất kỳ số lần di chuyển nào như vậy, chúng ta có thể chuyển đổi chuỗi thành một chuỗi dấu ngoặc đúng định dạng hay không, nghĩa là nó có thể biểu thị một biểu thức số học hợp lệ nếu chúng ta chèn vào.`+`Và`1`giữa các ký tự. 

Hệ quả quan trọng của thao tác được phép là dấu ngoặc đóng hoàn toàn có thể sắp xếp lại được miễn là chúng không di chuyển sang bên phải. Nói cách khác, tất cả`')'`các ký tự có thể được hoán vị tùy ý, nhưng chỉ theo cách chúng dịch chuyển sang trái so với vị trí ban đầu của chúng. Dấu ngoặc mở không thể di chuyển được. 

Kích thước đầu vào lên tới 200.000 ký tự. Bất kỳ cách tiếp cận bậc hai hoặc thậm chí gần bậc hai nào đối với chuỗi con hoặc mô phỏng lặp lại đều quá chậm. Quét tuyến tính hoặc quét tuyến tính với trạng thái bổ sung không đổi là cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi bắt đầu bằng nhiều dấu ngoặc đóng. Ví dụ,`"))(("`không thể sửa được vì không có thao tác nào có thể di chuyển dấu ngoặc đóng qua dấu ngoặc mở sang bên phải và tính hợp lệ của tiền tố về cơ bản bị phá vỡ khi bắt đầu. Một trường hợp cạnh khác là khi số lượng dấu ngoặc đóng không đủ “có thể phân phối” để khớp các lần mở theo thứ tự tiền tố mặc dù số lượng khớp với nhau trên toàn cầu. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng tất cả các cách có thể để di chuyển dấu ngoặc đóng. Mỗi thao tác chọn một`')'`và chèn nó sớm hơn, vì vậy về mặt lý thuyết, chúng ta có thể cố gắng tạo ra tất cả các hoán vị của dấu ngoặc đóng với ràng buộc là chúng không thể di chuyển sang phải. Điều này dẫn đến một không gian trạng thái có số mũ theo số dấu ngoặc đóng. Ngay cả việc cố gắng sửa lỗi cục bộ một cách tham lam vẫn sẽ yêu cầu quét và cập nhật nhiều lần, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là thao tác này sẽ loại bỏ mọi hạn chế về vị trí đối với dấu ngoặc đóng ngoại trừ việc chúng không thể vượt qua bên phải. Điều này có nghĩa là chúng ta được phép sắp xếp lại tất cả`')'`các ký tự tự do với nhau, miễn là nhiều vị trí mà chúng chiếm giữ tôn trọng rằng chúng ở một nơi nào đó không đúng với lần xuất hiện ban đầu của chúng. Vì chúng ta chỉ được hỏi liệu một chuỗi ngoặc đúng có thể được hình thành hay không, nên câu hỏi thực sự duy nhất là liệu chúng ta có thể gán đủ dấu ngoặc đóng đủ sớm để đáp ứng sự cân bằng tiền tố hay không. 

Điều kiện chuỗi ngoặc đúng sẽ rút gọn thành quy tắc tiền tố cổ điển: mỗi tiền tố phải có ít nhất bằng`'('`BẰNG`')'`và tổng số phải khớp. 

Vì chúng ta có thể “chuyển” dấu ngoặc đóng sang trái một cách hiệu quả, trở ngại duy nhất đến từ các tiền tố chứa quá nhiều dấu ngoặc đóng so với các mở có sẵn ở bất kỳ đâu để cung cấp chúng sớm. Cấu trúc sụp đổ để kiểm tra xem chuỗi có thể được sắp xếp lại thành một từ Dyck hợp lệ hay không khi chúng ta không thể di chuyển`'('`. 

Điều này dẫn đến việc kiểm tra tham lam: mô phỏng việc đọc chuỗi, theo dõi các phần mở có sẵn và đảm bảo chúng ta không bao giờ cần nhiều lần đóng hơn mức có thể được cung cấp từ tổng số hậu tố còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp lại Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra tính khả thi của tiền tố tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề này thành một bài kiểm tra tính khả thi đối với số dư tiền tố dưới sự chuyển động hạn chế của dấu ngoặc đóng. 

1. Đếm tổng số`'('`Và`')'`. Nếu chúng không bằng nhau, trả về "Không". Một chuỗi ngoặc đúng yêu cầu các số bằng nhau và không có thay đổi thao tác nào được tính. 
2. Lặp lại chuỗi từ trái sang phải trong khi vẫn duy trì số lượng dấu ngoặc mở mà chúng ta đã thấy cho đến nay. Điều này thể hiện có bao nhiêu điều chưa từng có`'('`có khả năng sẵn có để hỗ trợ các dấu ngoặc đóng trong tương lai. 
3. Duy trì bộ đếm xem chúng ta đã "cố định" bao nhiêu dấu ngoặc đóng vào các vị trí hợp lệ. Thay vì suy nghĩ về chuyển động thực tế, chúng tôi coi điều này là đảm bảo rằng ở mọi tiền tố, số lượng dấu ngoặc đóng bắt buộc không vượt quá số lượng có thể được cung cấp. 
4. Tại mỗi vị trí, hãy tính số lần mở chưa khớp tối thiểu mà chúng ta phải bảo toàn để duy trì giá trị trong tương lai. Nếu tại bất kỳ thời điểm nào chúng ta bị buộc phải rơi vào tình huống mà tiền tố được yêu cầu nhiều hơn`')'`hơn có sẵn`'('`, chúng tôi kết luận là không thể. 
5. Vì dấu ngoặc đóng có thể được dịch chuyển sang trái, nên bất kỳ sự thiếu hụt nào trong tiền tố đều không thể được sửa chữa nếu không có đủ lỗ mở trong tổng số vị trí hậu tố để bù đắp. Quá trình quét tham lam đảm bảo chúng tôi không bao giờ mở quá sớm. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tiền tố, số lượng có sẵn`'('`chính xác là số lần mở được thấy trừ đi số lần đóng mà chúng tôi đã khớp một cách hợp lý cho đến nay. Bởi vì chúng ta chỉ có thể di chuyển`')'`bên trái, bất kỳ cấu trúc hợp lệ nào cũng phải tôn trọng rằng không có tiền tố nào yêu cầu số lần đóng nhiều hơn số lần mở tồn tại trong toàn bộ chuỗi cho đến thời điểm đó. Nếu tiền tố vi phạm điều này thì không có sự sắp xếp lại nào có thể khắc phục được vì chúng ta không thể giới thiệu tiền tố mới.`'('`sớm hơn hoặc di chuyển chúng sang phải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    
    open_cnt = s.count('(')
    close_cnt = n - open_cnt
    
    if open_cnt != close_cnt:
        print("No")
        return
    
    balance = 0
    for ch in s:
        if ch == '(':
            balance += 1
        else:
            balance -= 1
        if balance < 0:
            print("No")
            return
    
    print("Yes")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên kiểm tra tính khả thi tổng thể thông qua số lượng, vì mọi chuỗi ngoặc hợp lệ đều phải có số lượng dấu ngoặc đơn mở và đóng bằng nhau. Sau đó, nó thực hiện quét cân bằng tiền tố tiêu chuẩn. Ý tưởng chính là mặc dù thao tác được phép, chúng tôi vẫn không thể sửa tiền tố trong đó số đóng vượt quá số lần mở vì không có thao tác nào có thể tạo các phần mở trước đó hoặc di chuyển các phần mở sang bên phải. Mô phỏng tiền tố nắm bắt chính xác sự cản trở này. 

Một điểm tinh tế là chúng tôi không mô phỏng rõ ràng các lần chèn lại được phép. Thao tác đó chỉ ảnh hưởng đến thứ tự tương đối của dấu ngoặc đóng, nhưng nó không thay đổi ràng buộc tiền tố cơ bản do các dấu mở đặt ra, do đó việc kiểm tra tính hợp lệ cổ điển vẫn đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`((()))`| tôi | char | cân bằng | 
| --- | --- | --- | 
| 1 | ( | 1 | 
| 2 | ( | 2 | 
| 3 | ( | 3 | 
| 4 | ) | 2 | 
| 5 | ) | 1 | 
| 6 | ) | 0 | 

Điều này cho thấy sự phát triển tiền tố hoàn toàn hợp lệ trong đó số dư không bao giờ giảm xuống dưới 0 và kết thúc ở mức 0. Nó xác nhận rằng không cần sắp xếp lại khi trình tự đã đúng. 

### Ví dụ 2:`())(()`| tôi | char | cân bằng | 
| --- | --- | --- | 
| 1 | ( | 1 | 
| 2 | ) | 0 | 
| 3 | ) | -1 (thất bại) | 

Ở vị trí 3, tiền tố trở nên không hợp lệ vì dấu ngoặc đóng xuất hiện khi không có phần mở nào phù hợp. Mặc dù các lần mở sau này tồn tại nhưng chúng không thể được di chuyển sang trái, vì vậy việc vi phạm tiền tố là vĩnh viễn. 

Điều này chứng tỏ rằng thuật toán loại bỏ chính xác các trường hợp trong đó cấu trúc ban đầu không thể cứu vãn được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua chuỗi với công việc không đổi trên mỗi ký tự | 
| Không gian | O(1) | Chỉ có quầy được duy trì | 

Các ràng buộc cho phép tối đa 200.000 ký tự, do đó, quét tuyến tính thoải mái trong giới hạn và không cần cấu trúc phụ trợ tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        s = sys.stdin.readline().strip()
        n = len(s)
        
        if s.count('(') != n - s.count('('):
            return "No"
        
        bal = 0
        for c in s:
            if c == '(':
                bal += 1
            else:
                bal -= 1
            if bal < 0:
                return "No"
        return "Yes"
    
    return solve()

# provided sample
assert run("((()))\n") == "Yes"

# minimum size
assert run("()\n") == "Yes"

# impossible due to prefix
assert run("())\n") == "No"

# all closing
assert run("))))((\n") == "No"

# already balanced but tricky order
assert run("()()()\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`| Có | Trường hợp hợp lệ tối thiểu | 
|`())`| Không | Lỗi tiền tố sớm | 
|`))))((`| Không | Mất cân bằng toàn cầu + hạn chế đặt hàng | 
|`()()()`| Có | Nhiều phân đoạn hợp lệ độc lập | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi chuỗi được cân bằng toàn cầu nhưng có vi phạm tiền tố sớm, chẳng hạn như`")(()"`. Thuật toán xử lý nó như sau: ở ký tự đầu tiên, số dư trở thành`-1`, gây ra sự từ chối ngay lập tức. Điều này phù hợp với thực tế là không có thao tác nào được phép có thể di chuyển dấu ngoặc mở trước vị trí 1, do đó không thể sửa được tiền tố. 

Một trường hợp khác là`"()())("`, trong đó tổng số bằng nhau nhưng tiền tố vẫn ngắt ở ký tự thứ năm. Quá trình quét đạt đến sự cân bằng`-1`tại thời điểm đó và thuật toán sẽ từ chối. Mặc dù sau này có thêm một lỗ mở nhưng nó không thể di chuyển sang trái đủ để khắc phục sự thiếu hụt ở ranh giới tiền tố. 

Trường hợp cạnh cuối cùng đã là các chuỗi đúng như`"((()))"`. Số dư không bao giờ giảm xuống dưới 0 và thuật toán chấp nhận mà không cần xem xét hoạt động sắp xếp lại, phản ánh rằng hoạt động chỉ có liên quan khi có thể cần sắp xếp lại nhưng không mở rộng lớp chuỗi hợp lệ tiền tố khả thi.
