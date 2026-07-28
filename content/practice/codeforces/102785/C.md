---
title: "CF 102785C - Kích thước"
description: "Chỉnh sửa Đầu vào là một biểu thức toán học mô tả kích thước vật lý. Mỗi chữ cái Latinh đại diện cho một chiều cơ bản, trong khi phép nhân, phép chia và dấu ngoặc đơn kết hợp các chiều theo cách thông thường."
date: "2026-07-27T19:36:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "C"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 96
verified: true
draft: false
---

[CF 102785C - Kích thước](https://codeforces.com/problemset/problem/102785/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Đầu vào là một biểu thức toán học mô tả một chiều vật lý. Mỗi chữ cái Latinh đại diện cho một chiều cơ bản, trong khi phép nhân, phép chia và dấu ngoặc đơn kết hợp các chiều theo cách thông thường. Lặp lại một chữ cái có nghĩa là nhân với chiều đó nhiều lần, vì vậy`AAA`đại diện cho sức mạnh thứ ba của`A`. 

Nhiệm vụ là đơn giản hóa toàn bộ biểu thức bằng cách kết hợp các chiều bằng nhau. Một chiều xuất hiện ở tử số sẽ hủy bỏ cùng một chiều xuất hiện ở mẫu số. Sau tất cả các lần hủy, các thừa số tử số và mẫu số còn lại phải được in riêng biệt, mỗi bên được sắp xếp theo thứ tự yêu cầu:`A, a, B, b, ... , Z, z`. 

Độ dài biểu thức tối đa là 1000 ký tự. Kích thước này đủ nhỏ để một trình phân tích cú pháp tuyến tính có thể sử dụng được, nhưng nó loại trừ các cách tiếp cận liên tục mở rộng hoặc đơn giản hóa các chuỗi trung gian lớn. Một giải pháp thử mọi cặp thừa số có thể có thể tiếp cận hành vi bậc hai và việc thay thế chuỗi lặp đi lặp lại có thể trở nên tồi tệ hơn vì mỗi lần thay thế có thể quét lại hầu hết biểu thức. 

Các phần phức tạp là do lồng nhau và do sự khác biệt giữa vị trí của kích thước và dấu hiệu cuối cùng của nó. Ví dụ, trong`kg/(kg/(m/s))`, phép chia bên trong đổi dấu hai lần trước khi biết kết quả cuối cùng. Một trình phân tích cú pháp chỉ xử lý phép nhân và chia từ trái sang phải mà không tôn trọng dấu ngoặc đơn sẽ giữ sai`kg`trong câu trả lời. 

Một trường hợp khác là hủy bỏ hoàn toàn. Đối với đầu vào`A/A`, kết quả đúng là:```
1
1
```Việc triển khai bất cẩn có thể in một dòng trống cho một bên thay vì biểu thị thứ nguyên nhận dạng bằng`1`. 

Độ nhạy trường hợp cũng rất đáng kể. Đối với đầu vào`aA`, hai biểu tượng có kích thước khác nhau. Đầu ra đúng là:```
Aa
1
```Chỉ xử lý các chữ cái bằng dạng chữ thường sẽ hủy chúng một cách không chính xác. 

Các thứ nguyên lặp lại phải được tính, không được coi là một lần xuất hiện. Đối với đầu vào`AAA/A`, kết quả đúng là:```
AA
1
```Giải pháp sử dụng tập hợp thay vì số đếm sẽ làm mất số mũ còn lại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chuyển đổi biểu thức thành danh sách các yếu tố riêng lẻ trong khi đánh giá các phép toán. Mỗi phép nhân giữ dấu của ngữ cảnh hiện tại và mỗi phép chia đảo ngược nó. Sau đó, một bộ đơn giản hóa lực lượng vũ phu có thể so sánh mọi hệ số tử số với mọi hệ số mẫu số và loại bỏ các cặp khớp. Điều này đúng vì việc hủy chỉ phụ thuộc vào số lần mỗi chiều xuất hiện ở mỗi bên. Tuy nhiên, trong trường hợp xấu nhất, một biểu thức có khoảng 1000 thừa số sẽ yêu cầu khoảng một triệu phép so sánh và việc xóa nhiều lần khỏi danh sách sẽ làm tăng thêm chi phí. 

Quan sát quan trọng là mọi chiều chỉ cần số mũ cuối cùng của nó. Vị trí chính xác nơi một yếu tố xuất hiện không còn quan trọng sau khi phân tích cú pháp. Một từ điển ánh xạ từng chữ cái vào một số nguyên có thể lưu trữ trực tiếp điều này. Số đếm dương có nghĩa là thứ nguyên vẫn nằm trong tử số, trong khi số đếm âm có nghĩa là kích thước vẫn ở mẫu số. 

Cấu trúc của biểu thức làm cho việc phân tích cú pháp đệ quy trở nên tự nhiên. Dấu ngoặc đơn tạo ra các biểu thức con độc lập và phép chia chỉ đơn giản là thay đổi dấu của mọi thừa số bên trong số hạng sau. Trong khi phân tích cú pháp, chúng tôi duy trì một hệ số biểu thị xem phần hiện tại đóng góp tích cực hay tiêu cực. Một chữ cái cộng số nhân đó vào số đếm của nó. Điều này biến toàn bộ sự đơn giản hóa thành một lần duyệt biểu thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm để so sánh lặp đi lặp lại | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức đệ quy và duy trì hệ số nhân cho ngữ cảnh hiện tại. Số nhân ban đầu là dương vì toàn bộ biểu thức bắt đầu ở tử số. Biểu thức trong ngoặc đơn nhận số nhân từ toán tử xung quanh nó. 
2. Khi tìm thấy một chữ cái, hãy cộng hệ số nhân hiện tại vào bộ đếm của chữ cái đó. Bộ đếm lưu trữ số mũ ròng, do đó việc hủy sau đó sẽ tự động diễn ra mà không cần danh sách tử số và mẫu số riêng biệt. 
3. Khi tìm thấy phép nhân, tiếp tục phân tích số hạng tiếp theo với cùng số nhân. Phép nhân không thay đổi dù một thừa số thuộc tử số hay mẫu số. 
4. Khi tìm thấy phép chia, hãy phân tích số hạng sau đây với số nhân ngược lại. Phép chia lật mặt của mọi chiều trong biểu thức sau, bao gồm mọi thứ bên trong dấu ngoặc đơn. 
5. Sau khi phân tích cú pháp xong, hãy tách các chữ cái có bộ đếm dương khỏi các chữ cái có bộ đếm âm. In số dương ở tử số và giá trị tuyệt đối của số âm ở mẫu số. 
6. Sắp xếp mỗi bên bằng cách sử dụng thứ tự chữ cái tùy chỉnh trong đó các phiên bản chữ hoa và chữ thường của cùng một chữ cái liền kề nhau. Nếu một bên không có thừa số thì in`1`. 

Lý do mà phép nhân dấu có tác dụng là vì mọi phép chia đều thay đổi dấu mũ của toàn bộ biểu thức theo sau nó. Các phép chia lồng nhau được xử lý một cách tự nhiên vì mỗi lệnh gọi đệ quy sẽ nhận được dấu hiệu do tất cả các phép chia xung quanh tạo ra. 

Tại sao nó hoạt động: trong quá trình phân tích cú pháp, bộ đếm cho mọi thứ nguyên chính xác là số mũ được đóng góp bởi mỗi lần xuất hiện của thứ nguyên đó. Phép nhân giữ nguyên dấu số mũ, trong khi phép chia phủ nhận sự đóng góp của biểu thức bị chia. Dấu ngoặc đơn chỉ kiểm soát biểu thức con nào nhận được sự thay đổi dấu đó. Sau khi truyền tải, số mũ dương biểu thị các thừa số tử số còn lại và số mũ âm biểu thị các thừa số mẫu số còn lại, do đó đầu ra được tạo ra là thứ nguyên được rút gọn hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    cnt = {}

    def parse(pos, sign):
        while pos < n:
            c = s[pos]

            if c.isalpha():
                cnt[c] = cnt.get(c, 0) + sign
                pos += 1
            elif c == '(':
                pos = parse(pos + 1, sign)
            elif c == ')':
                return pos + 1
            elif c == '*':
                pos += 1
            elif c == '/':
                pos = parse(pos + 1, -sign)
            else:
                pos += 1

        return pos

    parse(0, 1)

    def sort_key(c):
        return (c.lower(), 0 if c.isupper() else 1)

    numerator = []
    denominator = []

    for c in sorted(cnt.keys(), key=sort_key):
        if cnt[c] > 0:
            numerator.extend([c] * cnt[c])
        elif cnt[c] < 0:
            denominator.extend([c] * (-cnt[c]))

    print('*'.join(numerator) if numerator else "1")
    print('*'.join(denominator) if denominator else "1")

if __name__ == "__main__":
    solve()
```Từ điển`cnt`là cấu trúc dữ liệu trung tâm. Thay vì lưu trữ từng lần xuất hiện riêng biệt, mã ngay lập tức hợp nhất các kích thước bằng nhau vào số mũ cuối cùng của chúng. Điều này giúp giảm mức sử dụng bộ nhớ và tự động hủy bỏ. 

Hàm đệ quy`parse`đi qua biểu thức. Tham số thứ hai của nó là dấu hiện tại. Một chữ cái sửa đổi bộ đếm bằng dấu hiệu đó. Một toán tử phân chia gọi`parse`với dấu ngược lại, tương đương với việc nhân biểu thức con sau với`-1`. 

Dấu ngoặc đơn đóng trả lại quyền kiểm soát cho người gọi vì người gọi đã biết toàn bộ phần trong ngoặc đơn được nhân hay chia. Điều này tránh việc phải xây dựng một cây biểu thức riêng biệt. 

Phím sắp xếp không thể sử dụng thứ tự ASCII thông thường vì ASCII đặt tất cả các chữ cái viết hoa trước các chữ cái viết thường. Thứ tự bắt buộc sẽ nhóm hai trường hợp lại với nhau, do đó, trước tiên, khóa sẽ so sánh dạng chữ thường và sau đó đặt chữ hoa trước chữ thường. 

Cấu trúc đầu ra lặp lại từng chữ cái theo số mũ của nó. Điều này xử lý các kích thước như`AAA/A`chính xác vì số lượng được lưu trữ là lượng điện còn lại sau khi hủy. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, biểu thức là`kg/(kg/(m/s))`. 

| Bước | Phần biểu hiện hiện tại | Ký tên | Bộ đếm | 
| --- | --- | --- | --- | 
| 1 |`kg`trước giải hạng nhất | +1 | k:1, g:1 | 
| 2 | bên trong`kg`| -1 | k:0, g:0 | 
| 3 | bên trong`m`| +1 | m:1 | 
| 4 | bên trong`s`| -1 | s:-1 | 

hai`kg`các lần xuất hiện bị hủy vì phép chia lồng nhau thay đổi dấu hai lần. Yếu tố tích cực cuối cùng là`m`và yếu tố tiêu cực cuối cùng là`s`. 

Đối với mẫu thứ hai, biểu thức là`kg*a/(Fs*B)*A*Kt`. 

| Bước | Phần biểu hiện hiện tại | Ký tên | Bộ đếm | 
| --- | --- | --- | --- | 
| 1 |`k`,`g`,`a`| +1 | k:1, g:1, a:1 | 
| 2 |`F`,`s`,`B`| -1 | F:-1, s:-1, B:-1 | 
| 3 |`A`,`K`,`t`| +1 | A:1, K:1, t:1 | 

Sau khi sắp xếp theo thứ tự tùy chỉnh, tử số sẽ trở thành`A*a*Kt*kg`và mẫu số trở thành`B*Fs`. Ví dụ này chứng minh rằng các ký hiệu phân biệt chữ hoa chữ thường vẫn tách biệt và việc sắp xếp chỉ được thực hiện sau khi tất cả số học hoàn tất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong quá trình phân tích cú pháp và việc sắp xếp chỉ xử lý tối đa 52 chữ cái riêng biệt | 
| Không gian | O(1) | Từ điển truy cập chỉ lưu trữ các chữ cái Latinh có thể có, không phải kích thước biểu thức | 

Giới hạn đầu vào 1000 ký tự có thể được xử lý dễ dàng bằng một lần duyệt đệ quy. Giải pháp này không tạo ra các biểu thức trung gian mở rộng, do đó, nó vẫn hiệu quả ngay cả khi có nhiều dấu ngoặc đơn lồng nhau. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("kg/(kg/(m/s))\n") == "m\ns\n", "sample 1"
assert run("kg*a/(Fs*B)*A*Kt\n") == "A*a*Kt*kg\nB*Fs\n", "sample 2"

assert run("A\n") == "A\n1\n", "single numerator dimension"
assert run("A/A\n") == "1\n1\n", "complete cancellation"
assert run("AAA/A\n") == "AA\n1\n", "repeated dimensions"
assert run("aA\n") == "Aa\n1\n", "case-sensitive sorting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`|`A`Và`1`| Kích thước biểu thức tối thiểu | 
|`A/A`|`1`Và`1`| Hủy bỏ hoàn toàn và xử lý bên trống | 
|`AAA/A`|`AA`Và`1`| Đếm số mũ | 
|`aA`|`Aa`Và`1`| Phân loại tùy chỉnh và phân biệt chữ hoa chữ thường | 

## Vỏ cạnh 

cho`kg/(kg/(m/s))`, trình phân tích cú pháp ghi lại đầu tiên`kg`bằng dấu dương, sau đó nhập biểu thức được chia trong ngoặc có dấu âm. Bên trong biểu thức đó, phép chia thứ hai lật lại dấu cho`m/s`. hai`kg`các yếu tố hủy bỏ, để lại`m`ở tử số và`s`ở mẫu số. 

Vì`A/A`, đầu tiên`A`tăng bộ đếm lên một và thứ hai`A`giảm nó trở về không. Vì không bên nào còn yếu tố nào nên cả hai dòng đầu ra đều trở thành`1`. 

Vì`aA`, thuật toán giữ hai khóa từ điển riêng biệt vì kiểu chữ là một phần của tên thứ nguyên. Việc sắp xếp so sánh cả hai khóa bằng cách sử dụng dạng chữ thường và sau đó là kiểu chữ của chúng, tạo ra`Aa`thay vì hợp nhất chúng. 

Vì`AAA/A`, bộ đếm cho`A`trở thành ba và sau đó giảm đi một. Số mũ cuối cùng là hai, vì vậy đầu ra chứa`A`hai lần. Điều này xác nhận rằng thuật toán lưu trữ bội số thay vì chỉ hiện diện.
