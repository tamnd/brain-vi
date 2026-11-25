---
title: "CF 996A - Trúng xổ số"
description: "Chúng ta được cấp một số nguyên duy nhất biểu thị tổng số tiền Allen muốn rút. Ngân hàng chỉ phân phát tiền mặt với các mệnh giá cố định: 1, 5, 10, 20 và 100."
date: "2026-06-17T00:01:29+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 996
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 492 (Div. 2) [Thanks, uDebug!]"
rating: 800
weight: 996
solve_time_s: 82
verified: true
draft: false
---

[CF 996A - Trúng xổ số](https://codeforces.com/problemset/problem/996/A) 

**Đánh giá:** 800 
**Tags:** dp, tham lam 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên duy nhất biểu thị tổng số tiền Allen muốn rút. Ngân hàng chỉ phân phát tiền mặt với các mệnh giá cố định: 1, 5, 10, 20 và 100. Nhiệm vụ là thể hiện chính xác số tiền nhất định bằng cách sử dụng các giá trị tờ tiền này đồng thời giảm thiểu tổng số tờ tiền được sử dụng. 

Đây không phải là vấn đề phân vùng với các đồng tiền tùy ý. Tập hợp các mệnh giá là cố định và nhỏ, và chúng tôi được yêu cầu đếm số phần nhỏ nhất có tổng giá trị mục tiêu. Mỗi mệnh giá có thể được sử dụng không giới hạn số lần. 

Ràng buộc đầu vào cho phép số tiền lên tới một tỷ. Điều đó ngay lập tức loại trừ mọi lập trình động trong phạm vi giá trị, vì ngay cả một DP tuyến tính theo n cũng sẽ yêu cầu tới 10^9 trạng thái, điều này là không khả thi. Bất kỳ giải pháp hợp lệ nào cũng phải hoạt động trong thời gian không đổi. 

Một nỗ lực ngây thơ có thể cố gắng khám phá sự kết hợp của các tờ tiền, ví dụ như thử mọi cách để chọn số lượng của từng mệnh giá. Ngay cả khi chúng ta chỉ xem xét số lượng năm loại tờ tiền, việc liệt kê thô bạo đối với số lượng có thể là 100, 20, 10, 5 và 1 sẽ nhanh chóng trở nên không giới hạn vì số lượng tờ 1 đô la có thể thay đổi tới 10^9. Ngay cả việc hạn chế kết hợp vẫn dẫn đến một không gian tìm kiếm không thực tế. 

Một trường hợp phức tạp phát sinh khi các mệnh giá nhỏ hơn được sử dụng quá mức trước các mệnh giá lớn hơn. Ví dụ: lấy quá nhiều số 20 khi tờ 100 sẽ làm giảm tổng số lượng dẫn đến kết cấu không tối ưu. Một giải pháp đúng phải tránh những lựa chọn dưới mức tối ưu cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các kết hợp hóa đơn có thể có với tổng số tiền bằng n, theo dõi số lượng hóa đơn tối thiểu trong số các kết hợp hợp lệ. Về mặt khái niệm, điều này đúng vì nó liệt kê toàn bộ không gian giải pháp. Tuy nhiên, số lượng các kết hợp tăng lên với chính n, vì số lượng tờ 1 đô la có thể thay đổi độc lập đến n. Ngay cả khi chúng tôi rời rạc hóa theo mệnh giá, không gian tìm kiếm tỷ lệ thuận với n/1 lựa chọn ở chiều xấu nhất, dẫn đến việc khám phá tuyến tính hoặc tệ hơn trên mỗi trạng thái. Điều này vượt xa giới hạn cho n lên đến 10^9. 

Quan sát quan trọng là các mệnh giá được cấu trúc theo một hệ thống kinh điển: mỗi tờ tiền lớn hơn là bội số hoặc gần bội số của những tờ tiền nhỏ hơn và việc tham lam lấy tờ tiền lớn nhất có thể ở mỗi bước không bao giờ cản trở một giải pháp tối ưu. Điều này là do 100, 20, 10 và 5 được căn chỉnh sao cho việc thay thế một tờ tiền lớn hơn bằng những tờ tiền nhỏ hơn luôn làm tăng số lượng tờ tiền. Ví dụ: một tờ 20 đô la luôn tốt hơn hai tờ 10 đô la và một tờ 10 đô la luôn tốt hơn hai tờ 5 đô la, v.v. 

Cấu trúc này làm giảm vấn đề liên tục trích xuất số lượng tờ tiền có giá trị cao tối đa từ số tiền còn lại, sau đó chuyển sang mệnh giá tiếp theo. Quá trình này trở thành một chuỗi đơn giản của các phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / O(n) | O(1) | Quá chậm | 
| Tối Ưu (Tham Lam) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các mệnh giá từ lớn nhất đến nhỏ nhất, luôn trích xuất càng nhiều tờ tiền có mệnh giá hiện tại càng tốt. 

### bước

1. Bắt đầu với mệnh giá lớn nhất, 100, và tính xem có bao nhiêu tờ 100 đô la phù hợp với số tiền còn lại bằng cách chia số nguyên. Trừ tổng giá trị của chúng khỏi số tiền. 
2. Chuyển sang tờ 20 đô la và lặp lại thao tác tương tự với số tiền còn lại. Bước này đúng vì mọi phần còn lại sau khi loại bỏ 100 đều nhỏ hơn 100, vì vậy việc sử dụng 20 là tối ưu cục bộ trong phạm vi đã giảm đó. 
3. Lặp lại thao tác trích tham lam tương tự đối với tờ 10 đô la. 
4. Lặp lại lần nữa với tờ 5 đô la. 
5. Cuối cùng, số tiền còn lại phải được thanh toán bằng tờ 1 đô la, vì vậy phần còn lại chính là số tờ 1 đô la. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mỗi mệnh giá là cách tốt nhất có thể để thể hiện giá trị của nó dưới dạng các tờ tiền nhỏ hơn. Bất kỳ nỗ lực nào nhằm thay thế tờ tiền lớn hơn bằng tờ tiền nhỏ hơn đều chỉ làm tăng hoặc duy trì số lượng tờ tiền khi sử dụng những tờ tiền thay thế giống hệt nhau chứ không bao giờ cải thiện được số lượng. Điều này tạo ra một cấu trúc đơn điệu: một khi chúng ta lấy càng nhiều tờ tiền lớn càng tốt thì không có sự kết hợp nào sau này của những tờ tiền nhỏ hơn có thể bù đắp để giảm tổng số tờ tiền được sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

bills = [100, 20, 10, 5, 1]
ans = 0

for b in bills:
    ans += n // b
    n %= b

print(ans)
```Giải pháp lặp lại các mệnh giá theo thứ tự giảm dần và liên tục trích xuất số lượng hóa đơn tối đa cho mỗi giá trị. Biến`ans`cộng dồn tổng số hóa đơn đã sử dụng. Mỗi bước giảm`n`với số tiền còn lại không thể được thể hiện bằng mệnh giá cao hơn. 

Thứ tự xử lý là rất quan trọng. Nếu những mệnh giá nhỏ hơn được xử lý trước thì những tờ tiền lớn hơn có thể không bao giờ được sử dụng ngay cả khi chúng làm giảm tổng số lượng. 

Phép chia số nguyên và phép tính modulo ở đây an toàn vì tất cả các giá trị vẫn nằm trong phạm vi số nguyên của Python và không có vấn đề về độ chính xác nào xảy ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 125 

| Bước | Mệnh giá | Hóa đơn đã nhận | Còn lại n | Tổng hóa đơn | 
| --- | --- | --- | --- | --- | 
| 1 | 100 | 1 | 25 | 1 | 
| 2 | 20 | 1 | 5 | 2 | 
| 3 | 10 | 0 | 5 | 2 | 
| 4 | 5 | 1 | 0 | 3 | 
| 5 | 1 | 0 | 0 | 3 | 

Dấu vết này cho thấy chiến lược tham lam phân hủy 125 một cách tự nhiên thành 100 + 20 + 5 như thế nào, tạo ra tối thiểu 3 tờ tiền. 

### Ví dụ 2: n = 68 

| Bước | Mệnh giá | Hóa đơn đã nhận | Còn lại n | Tổng hóa đơn | 
| --- | --- | --- | --- | --- | 
| 1 | 100 | 0 | 68 | 0 | 
| 2 | 20 | 3 | 8 | 3 | 
| 3 | 10 | 0 | 8 | 3 | 
| 4 | 5 | 1 | 3 | 4 | 
| 5 | 1 | 3 | 0 | 7 | 

Điều này chứng tỏ số tiền còn sót lại sau khi khai thác tham lam luôn được xử lý chính xác bởi các mệnh giá nhỏ hơn mà không cần phải quay lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Số lượng mệnh giá cố định, mỗi mệnh giá được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Việc tính toán không phụ thuộc vào n về độ sâu vòng lặp hoặc đệ quy. Ngay cả với n lên đến 10^9, thuật toán vẫn thực hiện một số phép tính số học không đổi, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    bills = [100, 20, 10, 5, 1]
    ans = 0
    for b in bills:
        ans += n // b
        n %= b
    return str(ans)

# provided sample
assert run("125\n") == "3"

# minimum input
assert run("1\n") == "1"

# exact large denomination
assert run("100\n") == "1"

# multiple of 20
assert run("80\n") == "4"

# mixed case
assert run("68\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp biên nhỏ nhất | 
| 100 | 1 | chính xác mệnh giá lớn | 
| 80 | 4 | xử lý nhiều mệnh giá giữa | 
| 68 | 7 | hỗn hợp tham lam phân hủy | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi số tiền nằm ngay dưới một mệnh giá lớn, ví dụ n = 99. Thuật toán lấy 0 tờ 100 đô la, sau đó chuyển sang tờ 20 đô la, tạo ra bốn tờ 20 (80), để lại 19, sau đó là một tờ 10, một tờ 5 và bốn tờ 1. Tổng cộng là 4 + 1 + 1 + 4 = 10 tờ tiền. Mọi nỗ lực ép buộc tờ 100 đô la sẽ không hợp lệ và mọi nỗ lực sử dụng quá mức các mệnh giá nhỏ hơn trước đó sẽ chỉ làm tăng số lượng. 

Một trường hợp khác là khi n chia hết chính xác cho một mệnh giá lớn chẳng hạn như 200. Thuật toán ngay lập tức chọn hai tờ 100 đô la và kết thúc không có phần dư, xác nhận rằng phép trích tham lam tạo ra một biểu diễn tối ưu chặt chẽ mà không cần các tờ tiền nhỏ hơn.
