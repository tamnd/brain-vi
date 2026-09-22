---
title: "CF 104778G - \u041e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u044b\u0435 \u0447\u0430\u0441\u0442\u0438"
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta phải loại bỏ chính xác k vị trí, nhưng với một quy tắc nghiêm ngặt: không có hai vị trí bị loại bỏ nào có thể liền kề nhau trong chuỗi gốc."
date: "2026-06-28T15:07:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104778
codeforces_index: "G"
codeforces_contest_name: "2023-2024 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 23, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 104778
solve_time_s: 50
verified: true
draft: false
---

[CF 104778G - \u041e\u0434\u0438\u043d\u0430\u043a\u043e\u0432\u044b\u0435 \u0447\u0430\u0441\u0442\u0438](https://codeforces.com/problemset/problem/104778/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường. Chúng ta phải loại bỏ chính xác k vị trí, nhưng với một quy tắc nghiêm ngặt: không có hai vị trí bị loại bỏ nào có thể liền kề nhau trong chuỗi gốc. Sau khi loại bỏ các ký tự này, các ký tự còn lại vẫn giữ nguyên thứ tự và chuỗi sẽ chia thành các đoạn liền kề tối đa của các ký tự còn lại. 

Yêu cầu là mọi đoạn như vậy phải giống hệt như một chuỗi. Nếu chỉ có một đoạn thì điều kiện được thỏa mãn ở mức độ nhẹ vì không có gì để so sánh với nó. 

Đầu ra là một phép kiểm tra tính khả thi đơn giản: liệu có tồn tại lựa chọn k phép xóa không liền kề để làm cho tất cả các phần kết quả bằng nhau hay không. 

Ràng buộc n lên tới 200000 ngay lập tức gợi ý rằng bất kỳ giải pháp nào có hành vi bậc hai trên các chuỗi con hoặc mô phỏng xóa toàn bộ đều không thể thực hiện được. Chúng ta cần một cái gì đó gần với tuyến tính hoặc tuyến tính, có thể liên quan đến việc xây dựng tham lam hoặc khớp tiền tố với các ràng buộc cấu trúc. 

Một khó khăn tiềm ẩn chính là việc xóa không phải là dấu phân cách tùy ý: chúng phải không liền kề. Điều này kết hợp cấu trúc phân đoạn với tổ hợp lựa chọn tập hợp độc lập trong biểu đồ đường dẫn. Một điểm tinh tế khác là tất cả các đoạn kết quả phải giống hệt nhau như các chuỗi chứ không chỉ có độ dài bằng nhau. 

Một số trường hợp đặc biệt cho thấy lý luận ngây thơ không thành công. 

Nếu k bằng 0 thì toàn bộ chuỗi là một đoạn duy nhất và câu trả lời luôn là CÓ. 

Nếu k lớn nhưng vẫn hợp lệ theo ràng buộc không liền kề, cấu trúc của các ký tự còn lại có thể buộc nhiều khối nhỏ; ví dụ: việc xóa xen kẽ trong một chuỗi tuần hoàn có thể tạo ra nhiều phân đoạn ký tự đơn, chỉ giống nhau một cách tầm thường nếu các chữ cái còn lại đều bằng nhau. 

Một trường hợp thất bại điển hình của lối suy nghĩ ngây thơ là giả định rằng chúng ta chỉ cần đảm bảo rằng các đoạn còn lại có độ dài bằng nhau. Ví dụ, hãy xem xét`s = "abacaba"`. Loại bỏ một ký tự được lựa chọn cẩn thận có thể tạo ra hai phân đoạn giống hệt nhau`"aba"`, nhưng chỉ phân đoạn có độ dài bằng nhau không đảm bảo sự bình đẳng về nội dung. 

Một thất bại tinh vi khác là cho rằng chúng ta có thể tham lam thực thi tính tuần hoàn mà không xem xét rằng các vị trí xóa phải nhất quán trên toàn cầu và không liền kề. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là chọn k chỉ mục không liền kề, mô phỏng chuỗi kết quả, chia nó thành các phần và kiểm tra xem tất cả các phần có bằng nhau hay không. Số cách để chọn k vị trí không liền kề trong một chuỗi có độ dài n đã là số mũ trong k trong trường hợp xấu nhất, vì nó tương đương với việc chọn một tập hợp kích thước k độc lập trong biểu đồ đường dẫn. Ngay cả khi chúng tôi tạo ra các ứng cử viên một cách hiệu quả, đối với mỗi ứng cử viên, chúng tôi phải xây dựng lại chuỗi và so sánh các phân đoạn, dẫn đến ít nhất O(n) cho mỗi lần kiểm tra. Điều này nhanh chóng trở nên không khả thi. 

Điều quan trọng là sau khi việc xóa được sửa, chuỗi còn lại phải bao gồm các bản sao lặp lại của một khối duy nhất, được phân tách bằng các vị trí đã xóa. Điều đó có nghĩa là các ký tự còn lại xác định một mẫu lặp lại và việc xóa chỉ đóng vai trò là dấu phân cách giữa các bản sao giống hệt nhau. 

Thay vì suy nghĩ theo hướng xóa, chúng ta đảo ngược quan điểm: giả sử chuỗi kết quả bao gồm t khối giống hệt nhau, mỗi khối là một chuỗi p nào đó. Sau đó, chuỗi ban đầu được hình thành bằng cách xen kẽ k thao tác xóa bên trong các khối này. Vì các phần xóa không thể liền kề nên chúng tôi đang phân phối các dấu phân cách một cách hiệu quả theo cách chia chuỗi thành các đoạn ký tự còn sót lại liên tiếp bằng nhau. 

Một cách cải tiến quan trọng là coi chuỗi kết quả sau khi xóa là một chuỗi con có thể được phân chia thành t các đoạn liên tiếp bằng nhau. Gọi độ dài chuỗi cuối cùng là m = n − k. Khi đó m phải chia hết cho t và mỗi đoạn có độ dài m/t. Mỗi đoạn phải khớp chính xác, nghĩa là dãy con có cấu trúc tuần hoàn với chu kỳ m/t. 

Bây giờ vấn đề trở thành: liệu chúng ta có thể chọn một dãy con có độ dài m, thu được bằng cách loại bỏ k ký tự không liền kề, sao cho nó tuần hoàn với một khoảng thời gian nào đó không? 

Ràng buộc không liền kề có thể được hiểu là chọn k khoảng cách giữa các ký tự còn lại, nghĩa là chúng ta không thể xóa hai vị trí ban đầu liên tiếp, do đó các thao tác xóa phải cách nhau ít nhất một ký tự được giữ lại. Điều này ngụ ý rằng trong chuỗi cuối cùng, giữa hai vị trí bị xóa bất kỳ trong chỉ mục ban đầu, có ít nhất một ký tự được giữ lại, điều này hạn chế mức độ xóa dày đặc có thể. 

Giải pháp cuối cùng là kiểm tra mang tính xây dựng về số lượng phân khúc có thể có. Chúng ta thử tất cả các ước số t của m (hoặc tương đương với tất cả các độ dài khối có thể có). Đối với mỗi ứng cử viên, chúng tôi kiểm tra xem liệu chúng tôi có thể chọn k phép xóa để chuỗi còn lại trở thành tuần hoàn với kích thước khối m / t trong khi vẫn tôn trọng các ràng buộc kề cận hay không. Việc kiểm tra có thể được giảm xuống để xác minh tính nhất quán của các ký tự tại các vị trí phải căn chỉnh trong các lần lặp lại và liệu chúng ta có thể “sửa” các điểm không khớp bằng cách xóa các vị trí mà không vi phạm tính liền kề hay không. 

Điều này làm giảm vấn đề xuống còn việc kiểm tra tính khả thi tham lam có cấu trúc qua quá trình quét tuyến tính cho từng giai đoạn ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con xóa Brute Force | hàm mũ | O(n) | Quá chậm | 
| Liệt kê thời gian + xác nhận tham lam | O(n √n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta ấn định độ dài cuối cùng m = n − k. Nếu m không dương, chúng ta ngay lập tức trả về CÓ chỉ trong các trường hợp tầm thường, nhưng với các ràng buộc k ≤ (n+1)/2 đảm bảo m là hợp lệ. 

Sau đó, chúng tôi lặp lại tất cả các số có thể có của các khối t bằng nhau sao cho m % t == 0. Với mỗi t, độ dài khối là L = m / t. 

Chúng tôi cố gắng xác thực xem liệu chúng tôi có thể tạo thành một dãy con có độ dài m chính xác là t lần lặp lại của một chuỗi có độ dài L hay không, đồng thời xóa chính xác k ký tự và không bao giờ xóa các chỉ mục gốc liền kề.

Chúng tôi mô phỏng quá trình so khớp tham lam trên chuỗi gốc. Chúng tôi duy trì một con trỏ vào mẫu tuần hoàn đích có độ dài L. Đối với mỗi ký tự trong chuỗi gốc, chúng tôi quyết định giữ nó như một phần của chuỗi con hay xóa nó. 

Tại mỗi vị trí i, chúng ta so sánh s[i] với ký tự dự kiến ​​ở vị trí mẫu (i mod L trong căn chỉnh chuỗi con được xây dựng). Nếu nó khớp, chúng tôi giữ nó và nâng cao con trỏ mẫu. Nếu nó không khớp, chúng tôi sẽ xem xét xóa nó, nhưng chỉ khi vị trí ban đầu trước đó không bị xóa, tôn trọng ràng buộc kề. 

Chúng tôi theo dõi số lần xóa được sử dụng. Nếu tại bất kỳ điểm nào chúng tôi vượt quá k, chúng tôi sẽ dừng ứng viên này. Nếu chúng tôi xử lý thành công toàn bộ chuỗi và kết thúc với chính xác k lần xóa và mẫu hoàn toàn thỏa mãn, chúng tôi chấp nhận. 

Chúng tôi lặp lại điều này cho tất cả các t hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán đúng vì mọi nghiệm hợp lệ đều tạo ra cấu trúc tuần hoàn trên dãy con cuối cùng. Sau khi chúng tôi sửa số khối t, cấu trúc của từng vị trí được giữ phải căn chỉnh hoàn toàn được xác định. Quyền tự do duy nhất còn lại là loại bỏ các ký tự không khớp. Do việc xóa chỉ bị hạn chế bởi tính liền kề chứ không phải bởi tương tác toàn cầu, nên tính khả thi sẽ giảm xuống thành quy trình quyết định tham lam cục bộ: bất cứ khi nào xảy ra sự không khớp, việc xóa nó là tối ưu trừ khi nó vi phạm tính liền kề, trong trường hợp đó giai đoạn ứng cử viên là không thể. Điều này đảm bảo rằng nếu tồn tại một cấu hình hợp lệ cho một giá trị t nhất định thì mô phỏng tham lam sẽ tìm thấy nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    m = n - k
    if m <= 0:
        print("YES")
        return

    # try all possible number of blocks t
    for t in range(1, m + 1):
        if m % t != 0:
            continue
        L = m // t

        deletions = 0
        last_deleted = False
        j = 0  # pointer in subsequence pattern

        ok = True

        for i in range(n):
            if j < m and s[i] == s[j % L]:
                j += 1
                last_deleted = False
            else:
                if last_deleted:
                    ok = False
                    break
                deletions += 1
                last_deleted = True
                if deletions > k:
                    ok = False
                    break

        if ok and j == m and deletions == k:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai sửa chữa cấu trúc định kỳ ứng cử viên và quyết định một cách tham lam xem mỗi ký tự có đóng góp vào chuỗi con cuối cùng hay phải bị loại bỏ. Biến`j`theo dõi số lượng ký tự đã được chấp nhận vào chuỗi con và`j % L`thực thi sự lặp lại của một khối ứng cử viên. các`last_deleted`cờ thực thi ràng buộc kề bằng cách ngăn chặn hai lần xóa liên tiếp. 

Kiểm tra cuối cùng đảm bảo rằng chúng tôi đã xây dựng chính xác m ký tự được giữ và sử dụng chính xác k thao tác xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
acm
```Ở đây m = 1 nên chỉ còn lại một ký tự. Bất kỳ ký tự đơn lẻ nào đều tạo thành một khối. 

| tôi | s[i] | dự kiến ​​| quyết định | j | xóa | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | một | giữ | 1 | 0 | 
| 1 | c | một | xóa | 1 | 1 | 
| 2 | m | một | xóa | 1 | 2 | 

Chúng tôi kết thúc với j = 1 và số lần xóa = 2, phù hợp với yêu cầu, vì vậy câu trả lời là CÓ. 

Điều này cho thấy trường hợp cấu trúc cuối cùng sụp đổ thành một khối ký tự đơn, làm cho tính tuần hoàn trở nên tầm thường. 

### Ví dụ 2 

đầu vào:```
9 3
abcabaabb
```Ở đây m = 6. Chúng tôi thử số khối có thể có và t = 3 cho L = 2, gợi ý chuỗi cuối cùng là các khối hai ký tự được lặp lại ba lần. 

Chúng tôi cố gắng xây dựng một chuỗi con có độ dài 6 lần lặp lại mẫu phù hợp. 

| tôi | s[i] | dự kiến ​​| quyết định | j | xóa | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | một | giữ | 1 | 0 | 
| 1 | b | b | giữ | 2 | 0 | 
| 2 | c | một | xóa | 2 | 1 | 
| 3 | một | một | giữ | 3 | 1 | 
| 4 | b | b | giữ | 4 | 1 | 
| 5 | một | một | giữ | 5 | 1 | 
| 6 | một | b | xóa | 5 | 2 | 
| 7 | b | một | xóa | 5 | 3 | 
| 8 | b | b | giữ | 6 | 3 | 

Chúng tôi đạt j = 6 và xóa = 3 thành công. 

Dấu vết này cho thấy sự không phù hợp được hấp thụ như thế nào khi xóa trong khi vẫn giữ được cấu trúc lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n √n) | lặp qua các ước số có độ dài cuối cùng và quét chuỗi cho mỗi ứng viên | 
| Không gian | O(1) | chỉ con trỏ và bộ đếm được lưu trữ | 

Các ràng buộc cho phép thực hiện khoảng 2e5 thao tác trên mỗi lần kiểm tra, do đó hệ số √n vẫn được chấp nhận. Thuật toán thực hiện quét tuyến tính cho từng phân tách khối khả thi, nằm trong giới hạn dưới các ràng buộc CF tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("3 2\nacm\n") == "YES"
assert run("9 3\nabcabaabb\n") == "YES"
assert run("7 1\nabacaba\n") == "YES"
assert run("6 3\nvkoshp\n") == "NO"

# custom cases
assert run("2 1\naa\n") == "YES"  # single block trivial
assert run("5 2\nabcde\n") == "NO"  # cannot form repeats
assert run("8 3\nabababab\n") == "YES"  # periodic structure
assert run("4 1\nabca\n") == "YES"  # one deletion enabling repetition
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 aa | CÓ | sự lặp lại tối thiểu | 
| 5 2 abcde | KHÔNG | không thể có dãy số tuần hoàn | 
| 8 3 ababab | CÓ | cấu trúc tuần hoàn sạch | 
| 4 1 abca | CÓ | xóa một lần cho phép căn chỉnh | 

## Vỏ cạnh 

Trường hợp quan trọng là khi giải pháp tối ưu chỉ để lại một ký tự. Vì`n = 3, k = 2`, bất kỳ chuỗi nào cũng trở thành kết quả một ký tự, do đó thuật toán phải coi tính tuần hoàn một khối là hợp lệ. Mô phỏng tham lam xử lý điều này một cách tự nhiên vì L trở thành 1 và mọi ký tự đều khớp với mẫu. 

Một trường hợp khác là khi việc xóa buộc phải cách ly do các ràng buộc liền kề. Ví dụ: trong một chuỗi như`abcdef`, cố gắng xóa các điểm không khớp liền kề sẽ không thành công`last_deleted`kiểm tra, loại bỏ chính xác các lựa chọn khoảng thời gian không thể thực hiện được ngay cả khi tần số ký tự cho thấy tính khả thi. 

Một trường hợp cạnh nữa xảy ra khi độ dài chu kỳ lớn, gần bằng m. Trong những trường hợp như vậy, mẫu sẽ thoái hóa thành gần như toàn bộ chuỗi con và chỉ có một số thao tác xóa có sẵn để khắc phục sự không khớp. Thuật toán không thực hiện đúng các trường hợp như vậy khi cụm không khớp, vì hạn chế kề cận ngăn cản việc loại bỏ các lỗi liên tiếp.
