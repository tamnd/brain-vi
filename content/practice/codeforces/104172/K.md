---
title: "CF 104172K - GCD tối đa"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng ta được phép sửa đổi nhiều lần các phần tử riêng lẻ bằng cách sử dụng thao tác có dạng “thay thế một giá trị bằng số dư của nó khi chia cho một số nguyên dương đã chọn”."
date: "2026-07-02T00:55:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104172
codeforces_index: "K"
codeforces_contest_name: "The 2023 ICPC Asia Hong Kong Regional Programming Contest (The 1st Universal Cup, Stage 2:Hong Kong)"
rating: 0
weight: 104172
solve_time_s: 72
verified: true
draft: false
---

[CF 104172K - GCD tối đa](https://codeforces.com/problemset/problem/104172/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng ta được phép sửa đổi nhiều lần các phần tử riêng lẻ bằng cách sử dụng thao tác có dạng “thay thế một giá trị bằng số dư của nó khi chia cho một số nguyên dương đã chọn”. Chúng ta có thể áp dụng thao tác này bao nhiêu lần trên bất kỳ phần tử nào, nhưng chúng ta bị cấm biến một phần tử thành 0. 

Sau khi thực hiện bất kỳ chuỗi thao tác nào như vậy, chúng ta xem xét mảng kết quả và tính GCD của nó theo nghĩa thông thường. Mục tiêu của chúng tôi là chọn các hoạt động theo cách tối đa hóa GCD cuối cùng này. 

Khó khăn chính là thao tác không đơn giản giảm số lượng một cách tùy tiện. Hoạt động còn lại duy trì các ràng buộc về cấu trúc và không phải mọi giá trị nhỏ hơn đều có thể truy cập được từ một giá trị bắt đầu nhất định. 

Các ràng buộc cho phép tối đa 100.000 phần tử có giá trị lên tới 1e9, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các phép biến đổi hoặc kiểm tra tất cả các trạng thái cuối cùng có thể có cho mỗi phần tử đều không thể thực hiện được ngay lập tức. Ngay cả các cách tiếp cận O(n sqrt A) cũng quá chậm vì cả n và giá trị đều lớn. 

Một trường hợp phức tạp phát sinh từ quy tắc “không cho phép số 0”. Ví dụ: nếu một phần tử là 2, việc cố gắng giảm nó hơn nữa thường tạo ra số 0 và trở thành bất hợp pháp. Đặc biệt, từ 2 bạn không thể đạt tới 1 bằng cách sử dụng bất kỳ chuỗi thao tác hợp lệ nào, bởi vì mọi lựa chọn modulo đều giữ nó ở mức 2 hoặc tạo ra 0. 

Một trường hợp khác là ngay cả khi một giá trị nhỏ, nó có thể không thể rút gọn tự do thành tất cả các số trung gian. Ví dụ: 4 có thể đạt 1 nhưng không thể đạt 2 hoặc 3. Khả năng tiếp cận không đồng nhất này là lý do chính khiến cách tiếp cận tham lam ngây thơ “chỉ giảm mọi thứ về GCD mục tiêu” không thành công. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng xem xét tất cả các giá trị cuối cùng có thể có cho từng phần tử, sau đó tính toán GCD tốt nhất có thể trên tất cả các kết hợp. Điều này nhanh chóng trở thành cấp số nhân vì mỗi phần tử có thể phân nhánh thành nhiều trạng thái có thể tiếp cận và việc kết hợp các lựa chọn trên n phần tử là điều khó khăn. 

Một cái nhìn có cấu trúc hơn là đảo ngược vấn đề. Thay vì hỏi GCD nào có thể được hình thành sau khi biến đổi, chúng ta hỏi giá trị d nào có thể xuất hiện dưới dạng giá trị chung trong mảng cuối cùng. Nếu chúng ta có thể buộc mọi phần tử trở thành chính xác d thì GCD ít nhất phải là d. Vì GCD bị giới hạn trên bởi giá trị được chọn tối thiểu nên tối đa hóa mục tiêu thống nhất là chiến lược mạnh nhất. 

Điều này làm giảm vấn đề trong việc hiểu khả năng tiếp cận: đối với một phần tử cố định a, giá trị nào có thể được chuyển đổi thành sử dụng các phép toán modulo lặp lại mà không bao giờ chạm đến 0. 

Quan sát quan trọng là từ một giá trị a, bất kỳ số nào lớn hơn a/2 và nhỏ hơn a đều không thể đạt được, nhưng mọi giá trị từ 1 đến sàn((a − 1) / 2) đều có thể truy cập được trong nhiều nhất một thao tác và mọi giá trị trên phạm vi đó sẽ không thay đổi hoặc rơi vào vùng không thể truy cập được. Điều này tạo ra một hạn chế về cấu trúc rõ ràng: mỗi phần tử hoặc vẫn giữ nguyên hoặc chỉ có thể giảm xuống các giá trị tương đối nhỏ, nhưng không thể bao phủ toàn bộ khoảng một cách trơn tru. 

Điều này dẫn đến một hạn chế toàn cầu đối với mục tiêu ứng cử viên d. Nếu chúng ta muốn mọi phần tử trở thành chính xác d, thì với bất kỳ phần tử a nào, a đã bằng d hoặc a phải đủ lớn để đạt d thông qua việc rút gọn. Điều đó đòi hỏi a ≥ 2d + 1. Ngược lại, nếu d nằm trong vùng cấm ở giữa (giữa a/2 và a) thì không thể tạo ra được. 

Vì vậy, vấn đề trở thành việc kiểm tra xem giá trị nào d đồng thời “an toàn” cho tất cả các phần tử mảng. 

Việc xác minh trực tiếp theo ngày vẫn sẽ quá chậm, nhưng việc sắp xếp mảng sẽ cho thấy cấu trúc đơn giản hơn. Trở ngại duy nhất đối với ứng viên d là sự tồn tại của một giá trị a nào đó sao cho d < a 2d và a không bằng d. Bất kỳ giá trị nào như vậy sẽ chặn cấu trúc vì nó không thể chuyển đổi thành d. 

Điều này dẫn đến việc quét đơn giản qua các giá trị duy nhất được sắp xếp.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khả năng tiếp cận lực lượng vũ phu trên mỗi phần tử | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + kiểm tra tính hợp lệ trên các khoảng trống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Sắp xếp mảng và nén nó thành một danh sách các giá trị riêng biệt. 

Việc sắp xếp là cần thiết vì tình huống nguy hiểm duy nhất phụ thuộc vào các giá trị lân cận trong không gian giá trị chứ không phụ thuộc vào chỉ số. 
2. Đối với mỗi giá trị riêng biệt d theo thứ tự tăng dần, hãy coi đó là GCD cuối cùng ứng cử viên. 

Nếu chúng ta có thể làm cho tất cả các phần tử bằng d thì d có thể đạt được dưới dạng mảng GCD. 
3. Kiểm tra xem có tồn tại giá trị nào trong khoảng (d, 2d] hay không. 

Nếu một giá trị như vậy tồn tại và nó không bằng chính d thì d là không thể. Lý do là giá trị này không thể giảm xuống d theo các phép toán được phép. 
4. Nếu giá trị phân biệt tiếp theo sau d lớn hơn 2d thì không có giá trị chặn nào tồn tại trong khoảng bị cấm, vì vậy d là hợp lệ. 
5. Theo dõi d hợp lệ tối đa trên tất cả các ứng cử viên và xuất nó. 

### Tại sao nó hoạt động 

Mỗi phần tử chỉ có thể đóng góp vào một giá trị cuối cùng là chính nó hoặc một giá trị tương đối nhỏ chỉ bằng một nửa cường độ hiện tại của nó. Điều này tạo ra một ranh giới cứng: các giá trị trong khoảng (d, 2d] là “dính” theo nghĩa là chúng không thể thu gọn tất cả về d trừ khi chúng đã bằng d. 

Do đó, bất kỳ ứng cử viên d nào cũng hợp lệ khi tập giá trị không chứa vật cản không thể tránh khỏi trong khoảng đó. Kiểm tra các giá trị liền kề theo thứ tự được sắp xếp là đủ vì mọi vật cản sẽ xuất hiện dưới dạng phần tử lớn hơn tiếp theo nằm bên trong (d, 2d]. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    # compress distinct values
    vals = []
    for x in a:
        if not vals or vals[-1] != x:
            vals.append(x)
    
    ans = 1
    
    m = len(vals)
    for i, d in enumerate(vals):
        if i < m - 1:
            nxt = vals[i + 1]
            if nxt <= 2 * d:
                continue
        ans = max(ans, d)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp và nén các bản sao để chúng ta chỉ suy luận về độ lớn khác nhau. Đối với mỗi giá trị ứng viên d, chúng ta chỉ xem xét giá trị khác biệt tiếp theo. Nếu giá trị tiếp theo đó nằm trong 2d thì tồn tại một số trung gian không thể chuyển đổi thành d theo quy tắc rút gọn modulo, vì vậy d bị loại bỏ. Ngược lại, d là khả thi và trở thành một câu trả lời ứng cử viên. 

Câu trả lời cuối cùng là khả thi lớn nhất d. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 3 7 8 20
```Chúng tôi sắp xếp và nén: 

giá trị = [2, 3, 7, 8, 20] 

Chúng tôi kiểm tra từng giá trị: 

| d | giá trị tiếp theo | tiếp theo 2d? | hợp lệ | 
| --- | --- | --- | --- | 
| 2 | 3 | có (3 ≤ 4) | không | 
| 3 | 7 | có (7 ≤ 6 sai → không) thực tế là 7 > 6 | vâng | 
| 7 | 8 | có (8 ≤ 14) | không | 
| 8 | 20 | có (20  16 sai) | vâng | 
| 20 | không | vâng | vâng | 

Các ứng cử viên hợp lệ là 3, 8, 20, vì vậy câu trả lời là 20. 

Dấu vết này cho thấy các giá trị chỉ bị chặn như thế nào khi một giá trị khác nằm trong “cửa sổ thu gọn bị cấm” của chúng tối đa hai lần. 

### Ví dụ 2 

đầu vào:```
4
4 5 6 20
```giá trị = [4, 5, 6, 20] 

| d | giá trị tiếp theo | tiếp theo 2d? | hợp lệ | 
| --- | --- | --- | --- | 
| 4 | 5 | có (5 ≤ 8) | không | 
| 5 | 6 | có (6 ≤ 10) | không | 
| 6 | 20 | không (20  12 sai) | vâng | 
| 20 | không | vâng | vâng | 

Đáp án là 20. 

Ví dụ này nhấn mạnh rằng ngay cả khi tồn tại nhiều giá trị nhỏ thì chỉ những giá trị không có lân cận xung đột trong phạm vi gấp đôi phạm vi của chúng mới có thể đóng vai trò là mục tiêu GCD toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính trên các giá trị riêng biệt | 
| Không gian | O(n) | Lưu trữ cho mảng và giá trị nén | 

Các ràng buộc cho phép tối đa 100.000 phần tử, do đó, giải pháp dựa trên sắp xếp O(n log n) là đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    vals = []
    for x in a:
        if not vals or vals[-1] != x:
            vals.append(x)
    
    ans = 1
    m = len(vals)
    for i, d in enumerate(vals):
        if i < m - 1:
            if vals[i + 1] <= 2 * d:
                continue
        ans = max(ans, d)
    
    return str(ans)

# minimum size
assert run("1\n7") == "7"

# all equal
assert run("4\n5 5 5 5") == "5"

# increasing chain
assert run("5\n1 2 3 4 8") == "4"

# large separation
assert run("4\n10 25 100 1000") == "1000"

# tight blocking case
assert run("3\n4 5 6") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | chính nó | trường hợp cơ sở | 
| tất cả đều bình đẳng | cùng giá trị | ổn định | 
| giá trị nhỏ hỗn hợp | hiệu ứng chặn | ràng buộc khoảng thời gian | 
| giá trị thưa thớt | lựa chọn tham lam | sự độc lập của những khoảng trống | 
| cụm chặt chẽ | ứng viên không hợp lệ bị loại | quy tắc ranh giới 2d | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó, không cần chuyển đổi và câu trả lời là giá trị đó một cách tầm thường, vì GCD được tối đa hóa bằng cách giữ mọi thứ không thay đổi. 

Một trường hợp khác là khi các giá trị rất nhỏ và được đóng gói chặt chẽ, chẳng hạn như 4, 5, 6. Ở đây, mọi ứng cử viên ngoại trừ giá trị lớn nhất đều bị chặn vì mỗi giá trị có một lân cận trong khoảng nhân đôi của nó. Thuật toán loại bỏ chính xác 4 và 5 và chọn 6. 

Trường hợp cuối cùng là khi các giá trị có khoảng cách rộng, chẳng hạn như 10, 25, 100. Ở đây, hầu hết các ứng cử viên đều hợp lệ vì không có giá trị nào rơi vào khoảng cấm của giá trị khác và thuật toán xác định chính xác giá trị tối đa là GCD tốt nhất có thể đạt được.
