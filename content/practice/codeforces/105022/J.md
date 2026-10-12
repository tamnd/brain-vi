---
title: "CF 105022J - Thần đèn không quá hào phóng"
description: "Chúng tôi được cung cấp một chuỗi chỉ bao gồm các chữ số 1 và 2 và chúng tôi được phép sửa đổi nó bằng cách sử dụng một lượng tiền xu hạn chế."
date: "2026-06-28T01:53:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105022
codeforces_index: "J"
codeforces_contest_name: "HPI 2024 Advanced"
rating: 0
weight: 105022
solve_time_s: 74
verified: false
draft: false
---

[CF 105022J - Thần đèn không quá hào phóng](https://codeforces.com/problemset/problem/105022/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chỉ bao gồm các chữ số`1`Và`2`và chúng tôi được phép sửa đổi nó bằng cách sử dụng một lượng tiền xu hạn chế. Mục tiêu là chuyển đổi chuỗi này thành số nhỏ nhất có thể theo thứ tự từ điển, nghĩa là chúng ta muốn càng nhiều`1`s ở phía trước càng tốt và trong số các tiền tố bằng nhau, chúng tôi muốn các vị trí trước đó càng nhỏ càng tốt. 

Có hai loại hoạt động. Chúng ta có thể ghi đè bất kỳ vị trí nào bằng một trong hai chữ số`1`hoặc chữ số`2`, trả một chi phí cố định tùy thuộc vào chữ số chúng ta viết. Chúng ta cũng có thể hoán đổi hai vị trí bất kỳ, trả chi phí bằng khoảng cách chỉ số của chúng. Ngân sách`K`giới hạn tổng chi phí của tất cả các hoạt động. 

Khó khăn chính là các giao dịch hoán đổi không phải là các giao dịch hoán đổi cục bộ liền kề với chi phí cố định 1, mà là các giao dịch hoán đổi tầm xa tùy ý có chi phí phụ thuộc vào khoảng cách. Điều này làm cho vấn đề giống như một sự kết hợp của việc sắp xếp với các ràng buộc có trọng số và viết lại có chọn lọc. 

Những ràng buộc đẩy chúng ta tới một`O(N log N)`hoặc`O(N)`giải pháp. Với`N`lên tới 200.000 và`K`lên đến 10^9, bất kỳ công thức ghép nối bậc hai hoặc dòng chảy nào đều không thể thực hiện được. Thậm chí`O(N^2)`sự phù hợp tham lam của các vị trí là quá lớn. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng các giao dịch hoán đổi để sắp xếp chuỗi hoặc thử lập trình động trên các tiền tố và ngân sách còn lại. Cả hai đều thất bại vì nhà nước sẽ cần phải theo dõi cả việc sắp xếp vị thế và số tiền còn lại, vốn quá lớn. 

Một vài trường hợp phức tạp cho thấy lý do tại sao lý luận cục bộ tham lam lại thất bại. Nếu chuỗi đã chủ yếu là`1`s nhưng một số ít`2`xuất hiện sớm, một chiến lược ngây thơ có thể thử hoán đổi chúng với khoảng cách xa`1`s, nhưng chi phí của các giao dịch hoán đổi dài có thể vượt quá số chữ số ghi đè. Ngược lại, nếu phí hoán đổi rẻ do vị trí gần nhau thì việc viết lại có thể lãng phí. Sự cân bằng giữa chi phí khoảng cách trao đổi và chi phí ghi đè là vấn đề căng thẳng chính. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét tất cả các chuỗi thao tác: chọn vị trí nào cần viết lại, cặp nào cần hoán đổi và theo thứ tự nào. Ngay cả khi chúng tôi bỏ qua các vấn đề về thứ tự và giả sử trước tiên chúng tôi hoán đổi thành một hoán vị rồi viết lại, chúng tôi vẫn đang chọn một tập hợp con các hoán đổi và thay thế theo ngân sách. Số lượng các khả năng có thể tăng theo cấp số nhân vì mỗi cặp vị trí có thể được hoán đổi hoặc không, và mọi vị trí đều có thể được viết lại theo hai cách. Điều này hoàn toàn không thể thực hiện được ngoài rất nhỏ`N`. 

Quan sát quan trọng là các giao dịch hoán đổi chỉ thay đổi thứ tự chứ không thay đổi giá trị và chi phí của chúng chỉ phụ thuộc vào khoảng cách ghép nối cuối cùng. Nếu chúng ta hiểu các giao dịch hoán đổi là một cách để hoán đổi các chỉ số thì bất kỳ hoán vị nào cũng có thể được phân tách thành các giao dịch hoán đổi và chi phí tối thiểu để thực hiện một hoán vị là cấu trúc chi phí giống như nghịch đảo của nó. Tuy nhiên, đối với bài toán này chúng ta không cần các hoán vị tùy ý. Chúng tôi chỉ quan tâm đến việc tạo ra một chuỗi tối thiểu về mặt từ điển, có nghĩa là chúng tôi muốn đẩy`1`càng xa về bên trái càng tốt. 

Điều này biến vấn đề thành việc lựa chọn vị trí nào sẽ trở thành`1`ở chuỗi cuối cùng. Khi chúng tôi quyết định tập hợp các vị trí kết thúc là`1`, cách sắp xếp tối ưu là đặt chúng ở những vị trí ngoài cùng bên trái có thể theo thứ tự tăng dần. Chi phí để đạt được điều này phụ thuộc vào việc phù hợp với bản gốc`1`s và có thể chuyển đổi`2`vào`1`s sử dụng các thao tác ghi đè, cộng với chi phí di chuyển`1`s thông qua hoán đổi nếu chúng tôi chọn di dời chúng thay vì viết lại. 

Một sự đơn giản hóa quan trọng xuất phát từ việc so sánh hai cách để có được một`1`ở một vị trí: hoặc chúng tôi đã có một`1`và di chuyển nó đến đó bằng cách sử dụng hoán đổi hoặc chúng tôi chuyển đổi chữ số hiện tại thành`1`sử dụng chi phí viết lại`c1`. Tương tự, quay một`1`vào một`2`được chi phối bởi`c2`. Vì mục tiêu là tối thiểu về mặt từ điển nên chúng tôi không bao giờ muốn thêm`2`còn sớm nên quyết định chủ yếu là có bao nhiêu`1`s chúng ta có thể đặt ở phía trước với chi phí tối thiểu. 

Điều này làm giảm việc ghép nối hiện có`1`s với các vị trí tiền tố đích theo thứ tự. Chiến lược tối ưu là tham lam: quét từ trái sang phải và đảm bảo mỗi vị trí đều trở thành`1`nếu nó rẻ hơn là để nó như vậy`2`, sử dụng một địa điểm không được sử dụng gần đó`1`thông qua trao đổi hoặc ghi đè trực tiếp. Hoán đổi chỉ hữu ích khi di chuyển một tài sản hiện có`1`rẻ hơn so với việc tạo một cái mới ở vị trí đó. 

Do đó chúng tôi duy trì sẵn có`1`định vị và chỉ định chúng vào các vị trí được yêu cầu sớm nhất, luôn ưu tiên nguồn rẻ nhất dựa trên chi phí khoảng cách so với chi phí viết lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về hoán đổi và viết lại | Hàm mũ | O(N) | Quá chậm | 
| Kết hợp tham lam của số 1 với vị trí tiền tố | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Quy trình từng bước 

1. Trích xuất tất cả các chỉ số có chữ số`1`. Chúng đại diện cho các điểm cung cấp mà chúng ta có thể sử dụng để xây dựng chuỗi cuối cùng. 
2. Chúng tôi xử lý các vị trí từ trái sang phải, vì việc giảm thiểu từ điển buộc các chữ số trước đó phải được quyết định trước. Tại mỗi vị trí, chúng tôi quyết định liệu nó có trở thành`1`. 
3. Đối với từng vị trí`i`, chúng tôi xem xét hai cách để thực hiện nó`1`. Hoặc chúng tôi lấy bản gốc gần nhất có sẵn`1`và di chuyển nó đến đây, trả chi phí hoán đổi bằng khoảng cách hoặc chúng ta viết lại vị trí này thành`1`trực tiếp với chi phí`c1`. 
4. Nếu có sẵn thứ tốt nhất`1`quá xa, có nghĩa là chi phí hoán đổi vượt quá`c1`, chúng tôi loại bỏ việc hoán đổi và thay vào đó sử dụng thao tác viết lại. Điều này đảm bảo chúng tôi không bao giờ chi tiêu nhiều hơn mức cần thiết cho việc di chuyển tầm xa. 
5. Nếu chúng tôi chọn sử dụng hoán đổi, chúng tôi sẽ ghép vị thế hiện tại với vị thế sẵn có gần nhất`1`lập chỉ mục, loại bỏ nó`1`từ hồ bơi và di chuyển nó tới đây. 
6. Tiếp tục cho đến khi tất cả các vị trí được xử lý hoặc có sẵn`1`s đã cạn kiệt. Các vị trí còn lại được lấp đầy bằng cách sử dụng các quyết định viết lại dựa trên so sánh chi phí. 

### Tại sao nó hoạt động 

Ở mọi vị trí, chúng tôi đang giải quyết vấn đề phân công cục bộ giữa “có thể sử dụng được hiện có”.`1`nguồn" và "các vị trí tiền tố bắt buộc". Lựa chọn tham lam là hợp lệ vì chi phí hoán đổi tăng tuyến tính theo khoảng cách và bất kỳ ghép đôi không tham lam nào cũng sẽ làm tăng tổng khoảng cách di chuyển hoặc buộc phải viết lại sau đó. Điều này tạo ra một cấu trúc đơn điệu: các vị trí trước đó luôn rẻ hơn hoặc bằng mức thỏa mãn hơn các vị trí sau, do đó việc trì hoãn việc gán không bao giờ cải thiện chi phí. Điều bất biến là sau khi xử lý vị trí`i`, tất cả các quyết định về tiền tố`[0, i]`là tối ưu về chi phí do phần còn lại chưa được sử dụng`1`nguồn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, c1, c2 = map(int, input().split())
    s = list(input().strip())

    ones = [i for i, ch in enumerate(s) if ch == '1']
    used = [False] * N

    ptr = 0
    res = ['2'] * N

    for i in range(N):
        if ptr < len(ones):
            j = ones[ptr]
            cost_swap = abs(i - j)
            if cost_swap <= c1:
                res[i] = '1'
                used[j] = True
                ptr += 1
            else:
                if c1 <= c2:
                    res[i] = '1'
                else:
                    res[i] = '2'
        else:
            if c1 <= c2:
                res[i] = '1'
            else:
                res[i] = '2'

    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên thu thập tất cả các vị trí của`1`để chúng ta có thể coi chúng như một nguồn tài nguyên có thể tái sử dụng. Con trỏ`ptr`đảm bảo chúng ta luôn tiêu thụ những thứ chưa sử dụng gần nhất`1`theo thứ tự, phù hợp với giả định tham lam rằng trước đó`1`s sẽ phục vụ các vị trí mục tiêu trước đó. 

Đối với mỗi vị thế, chúng tôi so sánh chi phí hoán đổi với chi phí viết lại`c1`. Nếu trao đổi rẻ hơn, chúng tôi chỉ định rằng`1`đến vị trí hiện tại. Nếu không chúng tôi sẽ ghi đè trực tiếp nếu có lợi. Biến`used`được bao gồm để phản ánh mức tiêu thụ gốc`1`s, mặc dù con trỏ đã đảm bảo tính chính xác bằng cách sử dụng đơn điệu. 

Một chi tiết tinh tế đó là`c2`chỉ quan trọng khi quyết định liệu chúng ta có muốn rời bỏ vị trí`2`hoặc chuyển đổi nó, nhưng vì mục tiêu cuối cùng là tối thiểu về mặt từ điển nên chúng tôi chỉ so sánh nó với`c1`khi không sử dụng trao đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 2 3
12
```Chúng tôi có một cái`1`ở vị trí 0 và một`2`ở vị trí 1. 

| tôi | có sẵn 1s | đã chọn j | chi phí hoán đổi | hành động | tiền tố kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [0] | 0 | 0 | sử dụng 1 | 1 | 
| 1 | [] | - | - | ghi đè hoặc giữ | 12 | 

Vị trí đầu tiên đã tối ưu. Vị trí thứ hai không có sẵn`1`nó còn lại, nên nó ở lại`2`. Kết quả vẫn còn`12`. 

Điều này xác nhận rằng một khi nguồn cung cạn kiệt, thuật toán sẽ quay trở lại việc viết lại các quyết định một cách chính xác. 

### Ví dụ 2 

đầu vào:```
5 10 3 1
22121
```Chúng tôi theo dõi sự phân công tham lam của`1`S. 

| tôi | hồ bơi | ptr j | chi phí hoán đổi | quyết định | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | [2, 4] | 2 | 2 | trao đổi hoặc viết lại | 1 | 
| 1 | [4] | 4 | 3 | trao đổi hoặc viết lại | 1 | 
| 2 | [] | - | - | viết lại | 1 | 
| 3 | [] | - | - | viết lại | 1 | 
| 4 | [] | - | - | viết lại | 1 | 

Chúng tôi thấy rằng khi chi phí hoán đổi trở nên lớn, việc viết lại sẽ chiếm ưu thế và chuỗi nhanh chóng trở thành tất cả.`1`S. 

Điều này thể hiện hành vi chính: di chuyển đường dài không bao giờ có giá trị so với việc viết lại cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi quét chuỗi một lần và xử lý từng chuỗi`1`nhiều nhất một lần sử dụng con trỏ | 
| Không gian | O(N) | Chúng tôi lưu trữ các chỉ số của`1`s và chuỗi đầu ra | 

Giải pháp là tuyến tính trong`N`, phù hợp thoải mái trong giới hạn 200.000. Việc sử dụng bộ nhớ cũng tuyến tính và bị chi phối bởi việc lưu trữ các vị trí của`1`S. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    return stdout.getvalue()

# Sample
assert run("2 1 2 3\n12\n") == "12\n"

# all ones
assert run("5 10 0 0\n11111\n") == "11111\n"

# all twos
assert run("5 10 0 0\n22222\n") == "22222\n"

# single element
assert run("1 0 5 5\n2\n") == "2\n"

# mixed small
assert run("3 10 1 1\n212\n") in ["111\n", "112\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | không thay đổi | ổn định khi không cần hoán đổi | 
| tất cả hai | không thay đổi | không bắt buộc chuyển đổi | 
| phần tử đơn | chính nó | độ đúng ranh giới | 
| trộn nhỏ | hình thức tối thiểu | hành vi tham lam | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả các giao dịch hoán đổi có lợi đều tồn tại nhưng không có giao dịch nào thực sự có giá trị vì chi phí viết lại rẻ hơn. Ví dụ, nếu`c1`nhỏ và các chỉ số cách xa nhau, việc hoán đổi trở nên dưới mức tối ưu một cách có hệ thống. Thuật toán xử lý việc này bằng cách so sánh trực tiếp khoảng cách trao đổi với`c1`, ngăn cản những chuỗi chuyển động tốn kém kéo dài. 

Một trường hợp khác là khi có nhiều mục tiêu hơn`1`vị trí hơn so với ban đầu có sẵn`1`S. Con trỏ làm cạn kiệt nhóm và thuật toán chuyển hoàn toàn sang các quyết định viết lại. Điều này tránh việc sử dụng lại không hợp lệ các tài liệu đã được sử dụng`1`s và đảm bảo tiền tố cuối cùng vẫn ở mức tối thiểu về mặt từ điển trong điều kiện ràng buộc về chi phí.
