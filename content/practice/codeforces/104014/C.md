---
title: "CF 104014C - \u0412\u0435\u043d\u0434\u043e\u043c\u0430\u0442"
description: "Chúng tôi được cung cấp một máy bán hàng tự động có chứa một bộ sưu tập các gói đồ ăn nhẹ, mỗi gói có tên và giá được biểu thị bằng rúp và kopeks. Người mua cũng có một số tiền cố định, cũng được thể hiện theo cách tương tự."
date: "2026-07-02T04:55:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104014
codeforces_index: "C"
codeforces_contest_name: "2022-2023 ICPC NERC, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u0438 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0440\u0435\u0433\u0438\u043e\u043d\u0430 \u0438 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438"
rating: 0
weight: 104014
solve_time_s: 47
verified: true
draft: false
---

[CF 104014C - \u0412\u0435\u043d\u0434\u043e\u043c\u0430\u0442](https://codeforces.com/problemset/problem/104014/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một máy bán hàng tự động có chứa một bộ sưu tập các gói đồ ăn nhẹ, mỗi gói có tên và giá được biểu thị bằng rúp và kopeks. Người mua cũng có một số tiền cố định, cũng được thể hiện theo cách tương tự. Nhiệm vụ là chọn một gói đồ ăn nhẹ có giá càng lớn càng tốt nhưng không vượt quá số tiền của người mua, với một ràng buộc bổ sung là người mua phải trả số tiền chính xác, nghĩa là không được phép thay đổi. 

Do đó, đầu vào biểu thị một vấn đề lựa chọn bị ràng buộc trên một danh sách các mặt hàng, trong đó mỗi mặt hàng có trọng số bằng số (giá của nó) và chúng ta phải tìm trọng số tối đa nhỏ hơn hoặc bằng ngân sách cố định. 

Cấu trúc của các ràng buộc gợi ý một giải pháp quét tuyến tính. Với tối đa 100.000 mục, mọi cách tiếp cận sắp xếp hoặc thực hiện tìm kiếm lặp lại cho mỗi mục vẫn có thể được chấp nhận trong O(N log N), nhưng có thể tránh được chi phí không cần thiết vì chúng tôi chỉ cần một ứng cử viên tốt nhất. Điều này ngay lập tức loại trừ mọi thứ bậc hai như so sánh theo cặp hoặc quét lồng nhau. 

Một phần tinh vi của vấn đề là phân tích định dạng tiền tệ. Các giá trị được đưa ra dưới dạng các chuỗi như “R,cc”, trong đó rúp và kopeks phải được chuẩn hóa thành một giá trị nguyên duy nhất. Việc triển khai bất cẩn có thể so sánh các chuỗi theo từ điển hoặc so sánh riêng rúp và kopeks mà không chuẩn hóa thích hợp, dẫn đến thứ tự không chính xác. 

Một số trường hợp đặc biệt quan trọng: 

Trường hợp một cạnh là khi không có mặt hàng nào có giá cả phải chăng. Ví dụ: nếu ngân sách là 10,00 và tất cả các mặt hàng đều có giá cao hơn mức đó thì kết quả đầu ra chính xác là “-1”. Một giải pháp ngây thơ khởi tạo ứng viên tốt nhất không chính xác (ví dụ: sử dụng chỉ mục 0 mà không kiểm tra khả năng chi trả) sẽ trả về không chính xác một món ăn nhẹ không hợp lệ. 

Một trường hợp khó khăn khác là khi nhiều mặt hàng có cùng mức giá phải chăng tối đa. Ví dụ: nếu hai mặt hàng đều có giá 50,00 và ngân sách là 50,00 thì cả hai mặt hàng đều hợp lệ. Quá trình triển khai có lỗi có thể ghi đè hoặc bỏ qua các ứng viên hợp lệ tùy thuộc vào logic so sánh, nhưng bất kỳ logic "lấy tối đa ngân sách" ổn định nào cũng hoạt động. 

Trường hợp cuối cùng là khi giá rất gần với ngân sách, đặc biệt là xung quanh ranh giới kopek, chẳng hạn như 99,99 so với 100,00. Phân tích cú pháp kopeks không chính xác có thể thay đổi so sánh theo hệ số 100 và âm thầm phá vỡ tính chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: chuyển đổi tất cả giá thành một đơn vị số nguyên duy nhất, sau đó quét từng mặt hàng và kiểm tra xem giá của nó có nằm trong ngân sách hay không. Nếu đúng như vậy, hãy so sánh nó với ứng viên tốt nhất được tìm thấy cho đến nay và giữ ở mức tối đa. 

Điều này có tác dụng vì bài toán rút gọn thành việc tìm phần tử lớn nhất dưới một ràng buộc. Tuy nhiên, bất kỳ chiến lược phức tạp nào hơn như sắp xếp đều không cần thiết vì chúng ta không cần đặt hàng vượt quá một giá trị khả thi tối đa duy nhất. 

Cách tiếp cận bạo lực đã chạy ở O(N), đây là cách tối ưu về độ phức tạp tiệm cận. Không có cấu trúc ẩn như tổng tiền tố hoặc tổ hợp; mỗi mục là độc lập. Sự cải thiện thực sự duy nhất so với nỗ lực ngây thơ là bình thường hóa cẩn thận các giá trị tiền tệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(N) | O(1) | Đã chấp nhận | 
| Sắp xếp rồi quét | O(N log N) | O(N) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tất cả các giá trị tiền tệ thành số nguyên đại diện cho tổng số kopeks, sau đó thực hiện một lần chuyển qua các mục trong khi theo dõi ứng cử viên hợp lệ nhất. 

1. Phân tích chuỗi ngân sách thành rúp và kopeks, sau đó tính tổng ngân sách bằng kopeks như sau:`budget = R * 100 + cc`. Điều này đảm bảo so sánh thống nhất trên tất cả các mặt hàng. 
2. Khởi tạo hai biến:`best_price = -1`Và`best_name = ""`. lính gác`-1`đảm bảo rằng bất kỳ mục hợp lệ nào sẽ thay thế nó. 
3. Đối với mỗi gói đồ ăn nhẹ, hãy phân tích chuỗi giá của nó thành cùng một định dạng số nguyên. Bước này phải giống với bước chuyển đổi ngân sách để duy trì tính chính xác. 
4. Kiểm tra xem giá đồ ăn nhẹ có thấp hơn hoặc bằng ngân sách hay không. Nếu không, hãy bỏ qua hoàn toàn vì không thể mua nó nếu không đổi. 
5. Nếu bữa ăn nhẹ có giá cả phải chăng và giá của nó cao hơn`best_price`, cập nhật cả hai`best_price`Và`best_name`với mặt hàng này. Bản cập nhật tham lam này hoạt động vì chúng tôi chỉ quan tâm đến giá trị khả thi tối đa. 
6. Sau khi xử lý tất cả các mục, xuất ra`best_name`nếu nó được cập nhật ít nhất một lần, nếu không thì xuất ra “-1”. 

### Tại sao nó hoạt động 

Ở mỗi bước quét,`best_price`cửa hàng có mức giá phải chăng lớn nhất được thấy cho đến nay. Bởi vì chúng tôi chỉ cập nhật nó khi gặp một giá trị hợp lệ lớn hơn rất nhiều nên nó luôn là giá trị tối đa so với tiền tố được xử lý. Khi quá trình quét kết thúc, tiền tố là toàn bộ mảng, do đó giá trị được lưu trữ là giá trị tối đa toàn cầu trong giới hạn ngân sách. Không hoạt động nào trong tương lai có thể làm mất hiệu lực các so sánh trước đó vì tất cả các mục đều độc lập và không có tác dụng phụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_money(s):
    # format: R,cc
    r, cc = s.strip().split(',')
    return int(r) * 100 + int(cc)

n, budget_str = input().split()
n = int(n)
budget = parse_money(budget_str)

best_price = -1
best_name = ""

for _ in range(n):
    parts = input().split()
    name = parts[0]
    price = parse_money(parts[1])

    if price <= budget and price > best_price:
        best_price = price
        best_name = name

print(best_name if best_price != -1 else -1)
```Cốt lõi của giải pháp là`parse_money`hàm chuẩn hóa tất cả các giá trị thành một số nguyên có thể so sánh được. Nếu không có bước này, việc so sánh các giá trị đồng rúp-kopek hỗn hợp sẽ yêu cầu logic từ điển hoặc bộ dữ liệu, dễ xảy ra lỗi hơn khi triển khai thủ công. 

Quá trình quét duy trì một ứng cử viên tốt nhất, chỉ được cập nhật khi tìm thấy mức giá phải chăng tốt hơn. Điều này đảm bảo tính chính xác mà không cần sắp xếp hoặc cấu trúc dữ liệu bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 89,54
ChipsIT 69,69
YaChips 99,09
noChips 0,00
```Chúng tôi chuyển đổi ngân sách thành 8954 kopeks. 

| Bước | Tên | Giá (kopeks) | Giá cả phải chăng | Giá tốt nhất | Tên Hay Nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ChipIT | 6969 | Có | 6969 | ChipIT | 
| 2 | YaChips | 9909 | Không | 6969 | ChipIT | 
| 3 | noChips | 0 | Có | 6969 | ChipIT | 

Câu trả lời cuối cùng là`ChipsIT`. 

Dấu vết này cho thấy thuật toán bỏ qua các mục không thể chấp nhận được và duy trì mức tối đa trong số các mục hợp lệ. 

### Ví dụ 2 

đầu vào:```
4 50,00
A 10,00
B 50,00
C 49,99
D 60,00
```Ngân sách là 5000 kopeks. 

| Bước | Tên | Giá | Giá cả phải chăng | Giá tốt nhất | Tên Hay Nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | A | 1000 | Có | 1000 | A | 
| 2 | B | 5000 | Có | 5000 | B | 
| 3 | C | 4999 | Có | 5000 | B | 
| 4 | D | 6000 | Không | 5000 | B | 

Câu trả lời cuối cùng là`B`. 

Điều này thể hiện việc xử lý đúng sự bình đẳng ở ranh giới ngân sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi mục được xử lý một lần với phân tích cú pháp và so sánh O(1) | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì 100.000 phép tính số nguyên đơn giản và phân tách chuỗi có thể được xử lý dễ dàng trong vòng 2 giây bằng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def parse_money(s):
        r, cc = s.strip().split(',')
        return int(r) * 100 + int(cc)

    n, budget_str = input().split()
    n = int(n)
    budget = parse_money(budget_str)

    best_price = -1
    best_name = ""

    for _ in range(n):
        parts = input().split()
        name = parts[0]
        price = parse_money(parts[1])
        if price <= budget and price > best_price:
            best_price = price
            best_name = name

    return str(best_name if best_price != -1 else -1)

# sample
assert run("""3 89,54
ChipsIT 69,69
YaChips 99,09
noChips 0,00
""") == "ChipsIT"

# minimum case
assert run("""1 10,00
A 10,00
""") == "A"

# no affordable
assert run("""2 5,00
A 10,00
B 20,00
""") == "-1"

# boundary kopeks
assert run("""3 1,00
A 0,99
B 1,01
C 1,00
""") == "C"

# multiple optimal
assert run("""3 50,00
A 50,00
B 50,00
C 10,00
""") in ["A", "B"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 mục khớp chính xác | A | Độ chính xác của phần tử đơn | 
| Không có mặt hàng giá cả phải chăng | -1 | Xử lý trường hợp thất bại | 
| Ranh giới Kopek | C | Phân tích số đúng | 
| Nhân đôi giá trị tối ưu | A hoặc B | Sự ổn định dưới mối quan hệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các mặt hàng đều đắt hơn ngân sách. Ví dụ: với ngân sách 10,00 và các mục 20,00 và 30,00, thuật toán không bao giờ cập nhật`best_price`, vẫn ở mức -1. Kết quả đầu ra chính xác trở thành “-1” vì không có ứng cử viên hợp lệ nào được chọn. 

Một trường hợp cạnh khác xảy ra ở sự bằng nhau chính xác. Nếu ngân sách là 50,00 và một mặt hàng có giá chính xác là 50,00, thì điều kiện`price <= budget`đảm bảo nó được chấp nhận. Bản cập nhật quét`best_price`và điều này trở thành câu trả lời cuối cùng trừ khi một mục hợp lệ bằng hoặc lớn hơn xuất hiện sau đó. 

Trường hợp cạnh thứ ba là phân tích cú pháp chính xác. Đối với đầu vào như 0,05, việc không đệm hoặc diễn giải kopeks thành giá trị hai chữ số sẽ coi nó là 5 thay vì 0,05 rúp, vi phạm các so sánh. Việc chuẩn hóa số nguyên thành kopeks đảm bảo rằng 0,05 trở thành 5 một cách nhất quán và các phép so sánh vẫn chính xác.
