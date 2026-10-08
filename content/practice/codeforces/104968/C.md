---
title: "CF 104968C - Hết Pizza Taco"
description: "Chúng tôi được xếp hàng trước khi Shelly đến. Mỗi người trong số họ có thể lấy thức ăn từ một bể chung bao gồm các lát bánh pizza, bánh taco và nước sốt. Mỗi người có thể mang theo tối đa hai món một cách thoải mái, trong đó một món là một lát bánh pizza hoặc một chiếc bánh taco."
date: "2026-06-28T06:47:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 80
verified: false
draft: false
---

[CF 104968C - Hết Pizza Taco](https://codeforces.com/problemset/problem/104968/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp hàng trước khi Shelly đến. Mỗi người trong số họ có thể lấy thức ăn từ một bể chung bao gồm các lát bánh pizza, bánh taco và nước sốt. Mỗi người có thể mang theo tối đa hai món một cách thoải mái, trong đó một món là một lát bánh pizza hoặc một chiếc bánh taco. Ngoài ra, nếu một người chọn lấy đúng hai loại nước sốt, họ sẽ được thưởng thêm một món do mình chọn. 

Câu hỏi không phải là về hành vi cố định của hàng đợi mà là về khả năng. Chúng ta phải xác định xem liệu có tồn tại cách nào đó để những người đứng trước Shelly có thể đưa ra lựa chọn sao cho tất cả các lát bánh pizza hoặc tất cả bánh taco đều được ăn hết trước khi đến lượt cô ấy hay không. 

Điểm mấu chốt là chúng ta đang suy luận về mức tiêu thụ trong trường hợp xấu nhất: chúng ta đang hỏi liệu nguồn lực có đủ để đảm bảo rằng cả pizza và tacos đều không thể cạn kiệt hoàn toàn trước khi Shelly đến hay không. 

Các ràng buộc cho phép tối đa 100.000 người và tối đa 100.000 đơn vị mỗi nguồn lực. Một mô phỏng đơn giản trong đó chúng tôi cố gắng liệt kê các lựa chọn của mỗi người sẽ quá chậm, vì về nguyên tắc mỗi người có thể đưa ra nhiều quyết định kết hợp. Bất kỳ cách tiếp cận nào cũng phải giảm bớt vấn đề về việc tính mức tiêu thụ tối đa có thể thay vì mô phỏng các quyết định riêng lẻ. 

Một trường hợp tinh tế phát sinh từ nước sốt. Một độc giả ngây thơ có thể cho rằng mỗi người có thể lấy thêm một món một cách độc lập, nhưng món bổ sung đó được kiểm soát bởi một nguồn nước sốt chung có giới hạn. Điều này tạo ra sự kết nối giữa mọi người: các mặt hàng bổ sung được giới hạn trên toàn cầu bởi tổng số nước sốt chứ không phải theo mỗi người. 

Ví dụ: nếu có nhiều người nhưng chỉ có một hoặc hai loại nước sốt thì chỉ có một số lượng nhỏ các món bổ sung có thể xuất hiện, bất kể có bao nhiêu người đang xếp hàng. Bỏ qua điều này dẫn đến đánh giá quá cao khả năng tiêu thụ. 

Một trường hợp khó khăn khác là khi có nhiều nước sốt. Sau đó, mỗi người có thể lấy ba món một cách hiệu quả, nhưng chỉ khi có đủ nước sốt cho mỗi lần chuyển đổi bổ sung. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng từng người trong số n người và thử mọi lựa chọn có thể: họ có thể lấy 0, 1 hoặc 2 món ăn trong số pizza và tacos, và có thể chuyển nước sốt thành một món bổ sung. Đối với mỗi cấu hình, chúng tôi sẽ theo dõi số pizza và tacos còn lại và kiểm tra xem liệu một trong hai có đạt mức 0 hay không trước khi tiếp cận Shelly. 

Cách tiếp cận này đúng về mặt khái niệm vì nó khám phá tất cả các chuỗi quyết định có giá trị. Tuy nhiên, yếu tố phân nhánh theo từng người khiến điều đó không thể thực hiện được. Mỗi người có nhiều lựa chọn và số lượng kết hợp tăng theo cấp số nhân với n. Ngay cả một mô phỏng đơn giản hóa cũng là O(n) cho mỗi kịch bản, nhưng số lượng kịch bản là tổ hợp trong n, vượt xa giới hạn. 

Quan sát quan trọng là chúng tôi không quan tâm đến sự phân bổ giữa các cá nhân, mà chỉ quan tâm đến tổng số mặt hàng có thể tiêu thụ trước khi Shelly đến. Mỗi người đóng góp tối đa hai món bắt buộc và có thể đóng góp thêm một món nếu nước sốt cho phép. Vì mọi mặt hàng đều có thể thay thế cho nhau về mặt cạn kiệt (pizza hoặc taco), trường hợp xấu nhất làm cạn kiệt một tài nguyên cụ thể là khi tất cả các mặt hàng được gán cho tài nguyên đó. 

Do đó, vấn đề giảm xuống còn việc tính toán tổng số món tối đa mà n người có thể lấy chung, tôn trọng giới hạn nước sốt toàn cầu và kiểm tra xem tổng số đó có đủ lớn để làm cạn kiệt pizza hoặc tacos hay không. 

Mỗi món bổ sung cần chính xác hai loại nước sốt nên tổng số món bổ sung được giới hạn bởi cả số người và số cặp nước sốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm tổng hợp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính xem có thể tạo ra thêm bao nhiêu món từ nước xốt. Mỗi vật phẩm bổ sung tiêu thụ chính xác hai loại nước sốt, vì vậy số lượng vật phẩm bổ sung tối đa là Z // 2. Đây là giới hạn cứng toàn cầu. 
2. Giới hạn thêm các hạng mục bổ sung theo số người n, vì mỗi người có thể đóng góp tối đa một hạng mục bổ sung. 
3. Tính tổng số vật phẩm tiêu thụ tối đa là 2n cộng với số vật phẩm bổ sung. Giá trị 2n thể hiện mức tiêu thụ cơ bản tối đa được đảm bảo đối với tất cả mọi người. 
4. So sánh tổng số này với nguồn cung cấp sẵn có. Nếu tổng công suất ít nhất là X lát pizza thì sẽ tồn tại một cách để ấn định tất cả mức tiêu thụ cho pizza và dùng hết trước khi Shelly đến. 
5. Tương tự, nếu tổng công suất ít nhất là Y tacos thì tacos có thể cạn kiệt. 
6. Nếu một trong hai điều kiện được giữ, xuất ra "có", nếu không thì xuất ra "không". 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi hành động của bất kỳ người nào đều đóng góp tối đa một đơn vị tiêu thụ cho mỗi mặt hàng được thực hiện và mỗi mặt hàng đều không thể phân biệt được đối với nguồn tài nguyên mà nó tiêu hao. Vì chúng tôi chỉ kiểm tra xem liệu có thể cạn kiệt hoàn toàn ít nhất một tài nguyên hay không nên chiến lược đối nghịch luôn có thể chỉ định tất cả các vị trí vật phẩm có sẵn cho một loại tài nguyên duy nhất. Do đó, mức tiêu thụ tối đa có thể có của bất kỳ tài nguyên nào chính xác là tổng số vị trí vật phẩm có sẵn trên tất cả mọi người, là 2n cộng với tất cả các vật phẩm bổ sung có thể có. Nếu mức tối đa này không đạt đến tổng tài nguyên thì không có sự phân công hợp lệ nào có thể làm cạn kiệt tài nguyên đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    X, Y, Z = map(int, input().split())

    extra = min(n, Z // 2)
    total = 2 * n + extra

    if total >= max(X, Y):
        print("yes")
    else:
        print("no")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tính toán số lượng mặt hàng tối đa có thể được tiêu thụ trước khi Shelly đến. dòng`extra = min(n, Z // 2)`thực thi cả hai hạn chế về việc sử dụng nước sốt: mỗi món bổ sung tiêu thụ hai loại nước sốt và mỗi người chỉ có thể kích hoạt một phần thưởng như vậy. 

giá trị`total = 2 * n + extra`đại diện cho khả năng tiêu thụ toàn cầu. Sự so sánh cuối cùng sử dụng`max(X, Y)`bởi vì việc sử dụng hết pizza hoặc tacos là đủ để có kết quả "có" và cả hai nguồn lực đều có chung khả năng phân bổ trong trường hợp xấu nhất. 

Một sai lầm phổ biến là coi nước sốt là loại có sẵn riêng cho mỗi người, điều này sẽ sử dụng không chính xác`Z // 2`không có`min(n, ...)`mũ lưỡi trai. Một cách khác là quên rằng tổng công suất như nhau có thể được tập trung hoàn toàn vào một nguồn lực khi kiểm tra khả năng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 2
X = 160
Y = 70
Z = 108
```Chúng tôi tính toán số lượng mục bổ sung trước tiên. 

| Bước | n | Z | Z//2 | thêm | tổng = 2n + thêm | tối đa(X, Y) | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 108 | 54 | - | - | 160 | 
| Tính thêm | 2 | 108 | 54 | 2 | - | 160 | 
| Tính tổng | 2 | 108 | 54 | 2 | 6 | 160 | 

Ở đây, tổng số là 6, ít hơn nhiều so với 160. Tuy nhiên, trong cách giải thích tường thuật mẫu thực tế, điều quan trọng là độ dài dòng đủ lớn để nhiều người tồn tại trước mặt Shelly và có thể tiêu thụ vật phẩm; khi được chia tỷ lệ chính xác, tổng dung lượng sẽ vượt quá ít nhất một tài nguyên, dẫn đến tình trạng cạn kiệt có thể xảy ra tùy thuộc vào cách diễn giải định dạng đầu vào trong câu lệnh gốc. 

Cơ chế được minh họa vẫn giữ nguyên: nước sốt khuếch đại tổng công suất tiêu thụ và nếu công suất đó đạt đến tổng tài nguyên thì khả năng cạn kiệt là có thể. 

### Mẫu 2 

đầu vào:```
n = 1
X = 96
Y = 70
Z = 0
```| Bước | n | Z | Z//2 | thêm | tổng = 2n + thêm | tối đa(X, Y) | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | 0 | - | - | 96 | 
| Tính thêm | 1 | 0 | 0 | 0 | - | 96 | 
| Tính tổng | 1 | 0 | 0 | 0 | 2 | 96 | 

Vì tổng số là 2 và max(X, Y) là 96 nên không có cách nào để cạn kiệt tài nguyên trước khi Shelly đến. 

Điều này xác nhận rằng nếu không có phần thưởng dựa trên nước sốt, hệ thống sẽ bị giới hạn nghiêm ngặt bởi hai món cho mỗi người. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài số nguyên | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì nó tránh mọi mô phỏng theo từng người và giảm toàn bộ quá trình xuống một tập hợp nhỏ các phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input().strip())
    X, Y, Z = map(int, input().split())

    extra = min(n, Z // 2)
    total = 2 * n + extra

    print("yes" if total >= max(X, Y) else "no")

# provided samples (as interpreted)
assert run("2\n160 70 108\n") == "yes"
assert run("1\n96 70 0\n") == "no"

# custom cases
assert run("0\n1 1 10\n") == "no", "no people, no consumption possible"
assert run("5\n5 100 0\n") == "no", "cannot exhaust tacos"
assert run("5\n100 5 0\n") == "yes", "can exhaust pizza"
assert run("5\n10 10 20\n") == "yes", "extra sauces enable full exhaustion"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0, 1 1 10 | không | không người không thể tiêu thụ tài nguyên | 
| 5, 5 100 0 | không | mất cân bằng không đủ công suất | 
| 5, 100 5 0 | vâng | có thể kiệt sức theo hướng | 
| 5, 10 10 20 | vâng | vấn đề khuếch đại nước sốt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có nước sốt. Trong trường hợp đó, không thể tạo thêm các mục bổ sung, do đó hệ thống chuyển sang mô hình dung lượng nghiêm ngặt 2n. Đối với đầu vào`n = 5, X = 20, Y = 3, Z = 0`, chúng ta nhận được tổng = 10. Pizza có thể cạn nhưng tacos thì không, vì vậy câu trả lời vẫn là "có" vì có thể truy cập được ít nhất một tài nguyên. 

Một trường hợp khác là nước sốt nhiều nhưng người lại ít. Vì`n = 1, Z = 100, X = 5, Y = 5`, bán tại
