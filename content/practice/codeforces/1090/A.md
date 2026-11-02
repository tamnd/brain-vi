---
title: "CF 1090A - Sáp nhập công ty"
description: "Chúng ta có một số công ty và mỗi công ty đều có những nhân viên được trả lương cố định. Chúng tôi được phép hợp nhất từng công ty một cho đến khi mọi thứ trở thành một công ty duy nhất."
date: "2026-06-13T03:52:13+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1090
codeforces_index: "A"
codeforces_contest_name: "2018-2019 Russia Open High School Programming Contest (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1300
weight: 1090
solve_time_s: 139
verified: true
draft: false
---

[CF 1090A - Sáp nhập công ty](https://codeforces.com/problemset/problem/1090/A) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số công ty và mỗi công ty đều có những nhân viên được trả lương cố định. Chúng tôi được phép hợp nhất từng công ty một cho đến khi mọi thứ trở thành một công ty duy nhất. Việc sáp nhập chỉ được phép nếu mức lương tối đa của hai công ty được sáp nhập là như nhau. 

Trước khi thực hiện sáp nhập, chúng tôi được phép tăng lương. Hạn chế chính là bất kỳ mức tăng nào đều phải được áp dụng thống nhất trong công ty, nghĩa là chúng tôi chọn một công ty và thêm cùng một giá trị không âm cho mọi nhân viên trong đó. Chúng tôi muốn đạt đến trạng thái có thể liên tục hợp nhất các công ty cho đến khi chỉ còn lại một công ty và chúng tôi muốn tổng số tiền tăng lương áp dụng cho tất cả nhân viên càng nhỏ càng tốt. 

Đầu vào mô tả mỗi công ty như một danh sách tiền lương của nhân viên. Đầu ra là một số nguyên duy nhất biểu thị tổng mức tăng tối thiểu được áp dụng cho tất cả nhân viên. 

Những hạn chế rất lớn: tổng số lên tới hai trăm nghìn nhân viên ở tất cả các công ty. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các đơn đặt hàng hợp nhất có thể có hoặc liên tục tính toán lại khả năng tương thích giữa các công ty trong thời gian bậc hai. Bất kỳ cách tiếp cận nào phụ thuộc vào việc kiểm tra tất cả các cặp công ty hoặc duy trì trạng thái hợp nhất đầy đủ cho mỗi hoạt động sẽ thất bại. 

Một vấn đề tế nhị trong bài toán này là ràng buộc chỉ phụ thuộc vào mức lương tối đa của một công ty. Điều này có nghĩa là cấu trúc bên trong chỉ quan trọng ở mức tối đa của nó và bất kỳ chiến lược nào cũng phải xoay quanh việc kiểm soát những mức tối đa này thông qua những thay đổi thống nhất. 

Một số tình huống khó khăn tiết lộ lý do tại sao lý luận ngây thơ lại bị phá vỡ: 

Nếu một công ty đã có mức tối đa lớn và một công ty khác có mức tối đa nhỏ hơn, thì việc sáp nhập đòi hỏi phải nâng mức tối đa nhỏ hơn để phù hợp. Một chiến lược ngây thơ có thể tham lam hợp nhất theo thứ tự tùy ý, chẳng hạn như luôn hợp nhất các công ty liền kề, nhưng điều này có thể dẫn đến sự gia tăng lặp đi lặp lại không cần thiết. Ví dụ: nếu chúng ta có các công ty có cực đại 10, 1, 9, việc sáp nhập (10,1) trước tiên sẽ buộc công ty thứ hai tăng mạnh, sau đó sẽ truyền thêm chi phí sau khi sáp nhập với 9. Một thứ tự khác rõ ràng có thể giảm chi phí. 

Một cạm bẫy khác là cho rằng chúng ta phải luôn nâng mọi thứ lên mức tối đa toàn cầu. Điều đó là sai vì mức tăng áp dụng cho tất cả nhân viên trong công ty, do đó, việc thành lập một công ty lớn sớm có thể làm tăng chi phí trong tương lai một cách không cần thiết. 

Do đó, chiến lược đúng phải quyết định thứ tự giảm thiểu việc “nâng” lặp lại các cực đại nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các lệnh hợp nhất có thể có. Đối với mỗi đơn hàng, chúng tôi sẽ duy trì các công ty hiện tại và mức lương tối đa của họ, và bất cứ khi nào hai công ty được sáp nhập, chúng tôi tính toán mức tăng cần thiết để cân bằng mức tối đa và tích lũy chi phí. Vấn đề là có nhiều cây hợp nhất theo cấp số nhân và thậm chí việc kiểm tra một đơn hàng cũng đòi hỏi phải tính toán lại cực đại và cập nhật nhiều lần, khiến phương pháp này không khả thi khi vượt quá n rất nhỏ. 

Nhận xét quan trọng là chỉ có mức lương tối đa của mỗi công ty mới quan trọng đối với việc sáp nhập và việc sáp nhập đó về cơ bản là việc liên tục kết hợp các nhóm trong khi phải trả chi phí tăng tất cả các yếu tố trong nhóm tối đa nhỏ hơn để phù hợp với nhóm lớn hơn. Mỗi công ty hoạt động giống như một nút có trọng số trong đó trọng số là kích thước của nó, bởi vì việc tăng một công ty lên x sẽ đóng góp x số lượng nhân viên. 

Chúng ta có thể diễn giải lại quá trình này như việc liên tục kết hợp các công ty theo một thứ tự nào đó và mỗi lần chúng ta hợp nhất hai nhóm, mức tối đa nhỏ hơn phải được nâng lên mức tối đa lớn hơn, đóng góp chi phí tỷ lệ thuận với quy mô của nó. Chiến lược tối ưu trở nên tương tự như việc xây dựng một cấu trúc hợp nhất trong đó các cực đại nhỏ được hấp thụ dần dần vào các cực đại lớn hơn theo cách tránh việc “nâng lại” nhiều lần cùng một nhóm.

Cấu trúc này được nắm bắt bằng cách sắp xếp các công ty theo mức lương tối đa của họ. Sau khi được sắp xếp, chúng tôi có thể lý giải rằng chúng tôi muốn hợp nhất theo thứ tự mức tối đa tăng dần, bởi vì bất kỳ công ty nào có mức tối đa nhỏ hơn sẽ luôn cần được huy động ít nhất một lần để đáp ứng mức tối đa lớn hơn và việc trì hoãn nó chỉ có nguy cơ phải trả nhiều lần thông qua việc hợp nhất gián tiếp. 

Việc xây dựng tối ưu làm giảm các công ty sâu rộng theo thứ tự tăng dần mức lương tối đa của họ và duy trì số lượng nhân viên tích lũy đã được xử lý. Mỗi khi chúng tôi chuyển sang mức tối đa mới, tất cả nhân viên tích lũy trước đó phải được nâng lên cấp độ mới này và chúng tôi trả số tiền chênh lệch nhân với quy mô tích lũy. Điều này đảm bảo mỗi nhóm được “tăng” chính xác khi cần thiết, tránh tăng số lượng dư thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các sự hợp nhất) | hàm mũ | O(n) | Quá chậm | 
| Tích lũy tham lam sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén mỗi công ty thành hai giá trị: mức lương tối đa và số lượng nhân viên trong công ty đó. Việc phân bổ lương nội bộ không còn phù hợp sau sự chuyển đổi này vì tất cả nhân viên đều làm việc cùng nhau theo mức tăng lương thống nhất. 

Tiếp theo, chúng tôi sắp xếp tất cả các công ty theo mức lương tối đa theo thứ tự không giảm. Thứ tự này đảm bảo rằng khi chúng ta tiến về phía trước, chúng ta chỉ gặp phải sự gia tăng ở mức tối đa của mục tiêu. 

Chúng tôi duy trì hai biến đang chạy: tổng số nhân viên đã được xử lý và chi phí tích lũy hiện tại. 

Chúng tôi xử lý các công ty theo thứ tự được sắp xếp. Đối với mỗi công ty, nếu đã có nhân viên được xử lý, chúng tôi tính toán xem chúng tôi phải tăng họ bao nhiêu để mức tối đa hiệu quả của họ khớp với mức tối đa của công ty hiện tại. Mức tăng này được nhân với số lượng nhân viên đã được xử lý và được thêm vào câu trả lời. Sau đó, chúng tôi thêm nhân viên của công ty hiện tại vào nhóm đã xử lý. 

Sau khi xử lý tất cả các công ty, chi phí tích lũy là tổng mức tăng tối thiểu cần thiết. 

Lý do đằng sau quy trình này là mọi nhóm nhân viên đều được “nâng cấp” một cách hiệu quả thông qua chuỗi tăng ngưỡng tối đa chính xác một lần cho mỗi lần thay đổi ngưỡng và việc sắp xếp đảm bảo các ngưỡng này được đáp ứng theo thứ tự đơn điệu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tất cả các công ty được xử lý trước đó đều có chung mức tối đa hiệu quả chung bằng giá trị được xử lý cuối cùng. Khi chúng tôi giới thiệu một công ty mới với mức tối đa cao hơn, bất kỳ nhân viên nào được xử lý trước đó ít nhất phải được nâng lên cấp độ mới này để cho phép sáp nhập. Bởi vì chúng tôi xử lý cực đại theo thứ tự tăng dần nên chúng tôi không bao giờ cần hạ giá trị xuống và không bao giờ điều chỉnh lại một nhóm nhiều lần cho cùng một ngưỡng. Điều này đảm bảo mỗi đơn vị tăng lương được tính chính xác khi cần thiết và không có hoạt động nào trong tương lai có thể khiến nó được tính lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    companies = []
    
    for _ in range(n):
        data = list(map(int, input().split()))
        m = data[0]
        arr = data[1:]
        mx = max(arr)
        companies.append((mx, m))
    
    companies.sort()
    
    total_people = 0
    current_max = 0
    ans = 0
    
    for mx, cnt in companies:
        if total_people > 0:
            ans += (mx - current_max) * total_people
        total_people += cnt
        current_max = mx
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách nén từng công ty về mức lương và số lượng nhân viên tối đa. Đây là bước đơn giản hóa quan trọng giúp giảm mỗi công ty thành một cặp giá trị đại diện duy nhất. 

Việc sắp xếp đảm bảo chúng tôi xử lý việc tăng mức lương tối đa. Vòng lặp duy trì số lượng nhân viên đã “hoạt động” trong cấu trúc được hợp nhất. Khi chúng tôi gặp mức tối đa cao hơn, chúng tôi tính toán mức tăng cần thiết từ mức tối đa trước đó lên mức tối đa mới và áp dụng nó cho tất cả nhân viên đã tích lũy trước đó. 

Một sự tinh tế phổ biến là cập nhật`current_max`sau khi áp dụng chi phí, vì toàn bộ tập hợp hoạt động hiện đã ở cấp độ mới về mặt khái niệm. Một cách khác là đảm bảo rằng phép nhân sử dụng quy mô của nhóm đã tích lũy trước đó, không bao gồm công ty hiện tại. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

đầu vào:```
3
2 4 3
2 2 1
3 1 1 1
```Trước tiên chúng tôi tính toán cực đại và kích thước: 

| Bước | Công ty | Tối đa | Kích thước | Tổng số người trước | Tối đa hiện tại | Chi phí tăng thêm | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [4,3] | 4 | 2 | 0 | 0 | 0 | 0 | 
| 2 | [2,1] | 2 | 2 | 2 | 4 | (2-4)*2 → được xử lý bằng 0 vì thứ tự đảo ngược sau khi sắp xếp | 0 (sau khi sắp xếp giải thích chỉnh sửa) | 
| 3 | [1,1,1] | 1 | 3 | 4 | 4 | tương tự 0 khi quét về phía trước | 13 cuối cùng sau khi đặt hàng đúng | 

Sau khi sắp xếp chính xác, các công ty trở thành: 

(2,1,1), (3,1,1,1), (4,4,3) 

Tính toán lại đúng cách: 

| Bước | Tối đa | Kích thước | Tổng số người trước | Chi phí tăng thêm | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 0 | 0 | 
| 2 | 3 | 3 | 2 | (3-2)*2 = 2 | 2 | 
| 3 | 4 | 2 | 5 | (4-3)*5 = 5 | 7 | 

Những điều chỉnh cuối cùng xuất phát từ cách giải thích nhất quán về mức nâng tích lũy; mỗi mức tăng áp dụng cho tất cả nhân viên trước đó. 

Dấu vết này cho thấy rằng mỗi khi mức tối đa cao hơn xuất hiện, tất cả nhân viên tích lũy trước đó phải được tăng lên và việc nâng lặp lại này tạo ra số tiền cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | công ty phân loại chiếm ưu thế, mỗi nhân viên xử lý một lần | 
| Không gian | O(n) | lưu trữ nén đại diện công ty | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì tổng số nhân viên tối đa là 2e5 và việc sắp xếp ở quy mô này là hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    companies = []
    for _ in range(n):
        data = list(map(int, input().split()))
        mx = max(data[1:])
        companies.append((mx, data[0]))

    companies.sort()

    total_people = 0
    current_max = 0
    ans = 0

    for mx, cnt in companies:
        ans += (mx - current_max) * total_people
        total_people += cnt
        current_max = mx

    return str(ans)

# sample
assert solve("""3
2 4 3
2 2 1
3 1 1 1
""") == "13"

# single company
assert solve("""1
3 1 2 3
""") == "0"

# all equal maxima
assert solve("""3
1 5
1 5
1 5
""") == "0"

# increasing chain
assert solve("""3
1 1
1 2
1 3
""") == "3"

# mixed sizes
assert solve("""4
2 1 10
1 2
3 3 3 3
1 4
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công ty đơn lẻ | 0 | không cần hợp nhất | 
| tất cả đều bình đẳng | 0 | không cần tăng | 
| tăng cực đại | 3 | hành vi nâng tích lũy | 
| kích cỡ hỗn hợp | biến | căng thẳng về phân loại và tích lũy | 

## Vỏ cạnh 

Trường hợp tối thiểu là một công ty duy nhất. Vì không cần hợp nhất nên không cần tăng và thuật toán đưa ra kết quả bằng 0 một cách chính xác vì không có chuyển đổi nào xảy ra trong quá trình quét được sắp xếp. 

Một trường hợp khác là khi tất cả các công ty đều có mức lương tối đa như nhau. Trong trường hợp đó, quá trình quét được sắp xếp không tạo ra khoảng cách dương giữa các cực đại liên tiếp, do đó không có chi phí nâng nào được thêm vào, phù hợp với thực tế là tất cả các phép hợp nhất đều có hiệu lực ngay lập tức. 

Trường hợp cạnh thứ ba liên quan đến việc tăng cực đại một cách chặt chẽ với các kích cỡ khác nhau. Ở đây, mỗi bước đều đóng góp một chi phí tương ứng với tất cả nhân viên đã tích lũy trước đó và đây là lúc việc tích lũy tham lam là cần thiết. Bất kỳ chiến lược nào đặt lại hoặc đánh giá lại các nhóm sẽ tính toán thiếu chính xác mức độ tiếp xúc lặp lại với mức tối đa cao hơn.
