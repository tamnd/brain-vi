---
title: "CF 103486M - Trình tự"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu tính một giá trị dẫn xuất duy nhất dựa trên mức chênh lệch của nó. “Giá trị quan tâm” được định nghĩa là tích của hai đại lượng: độ dài của chuỗi và phạm vi của chuỗi, trong đó phạm vi là hiệu giữa…"
date: "2026-07-03T06:22:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "M"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 40
verified: true
draft: false
---

[CF 103486M - Trình tự](https://codeforces.com/problemset/problem/103486/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu tính một giá trị dẫn xuất duy nhất dựa trên mức chênh lệch của nó. “Giá trị quan tâm” được định nghĩa là tích của hai đại lượng: độ dài của chuỗi và phạm vi của chuỗi, trong đó phạm vi là hiệu giữa phần tử lớn nhất và nhỏ nhất. 

Vì vậy, nhiệm vụ giảm xuống còn đọc tất cả các giá trị, xác định phần tử tối thiểu và tối đa, tính toán chênh lệch của chúng và nhân chênh lệch đó với số phần tử. 

Các ràng buộc đủ nhỏ để chỉ cần quét tuyến tính một lần là đủ. Với tối đa 10.000 phần tử, bất kỳ giải pháp nào kiểm tra từng phần tử với số lần không đổi đều dễ dàng đủ nhanh. Ngay cả vài triệu thao tác nguyên thủy cũng không đáng kể dưới giới hạn 0,5 giây trong Python nếu được triển khai rõ ràng. 

Có một cạm bẫy nhỏ có thể gây ra việc triển khai bất cẩn: phạm vi số nguyên và xử lý dấu. Vì các giá trị có thể âm, giá trị tối thiểu có thể âm và giá trị dương tối đa và phạm vi có thể trở nên lớn. Ví dụ, nếu mảng là`[-100000, 100000]`, phạm vi là`200000`, và nhân với`n`phải được thực hiện sau khi tính toán các giá trị cực trị chính xác, không phải trong quá trình quét với các giả định từng phần. 

Một trường hợp cạnh khác là mảng một phần tử. Nếu như`n = 1`, thì cả giá trị tối đa và tối thiểu đều bằng nhau, phạm vi bằng 0 và câu trả lời phải bằng 0 bất kể giá trị đó là bao nhiêu. Bất kỳ triển khai nào khởi tạo không chính xác tối thiểu hoặc tối đa mà không sử dụng phần tử đầu tiên đúng cách có thể bị hỏng ở đây. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực có thể cố gắng tính toán phạm vi cho mọi cấu trúc con có thể hoặc tính toán lại các điểm cực trị nhiều lần, nhưng điều đó là không cần thiết vì trình tự đã cố định và chúng ta chỉ cần mức tối thiểu và tối đa toàn cục một lần. 

Ý tưởng đơn giản nhất là: đối với mỗi phần tử, hãy so sánh nó với tất cả các phần tử khác để xác định mức tối thiểu và tối đa. Điều đó dẫn đến cách tiếp cận O(n2) vì mỗi phần tử trong số n phần tử tham gia vào n phép so sánh. Mặc dù đúng nhưng nó vượt xa những gì cần thiết và lãng phí hoạt động bằng cách tính toán lại các thuộc tính chung giống nhau nhiều lần. 

Quan sát quan trọng là phạm vi đó chỉ phụ thuộc vào hai giá trị: mức tối thiểu và mức tối đa của toàn bộ chuỗi. Một khi những điều đó đã được biết thì không có cấu trúc nào khác quan trọng nữa. Điều này thu gọn vấn đề từ việc so sánh từng cặp thành một vấn đề tích lũy một lượt. 

Chúng tôi có thể duy trì hoạt động tối thiểu và tối đa trong khi quét mảng một lần. Mỗi phần tử mới cập nhật mức tối thiểu hoặc tối đa hiện tại theo thời gian không đổi. Sau khi quét, chúng tôi tính toán`(max - min) * n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Một lượt tối thiểu/tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, xác định số lượng giá trị chúng tôi sẽ xử lý. Điều này cũng xác định số nhân trong câu trả lời cuối cùng. 
2. Khởi tạo hai biến`mn`Và`mx`sử dụng phần tử đầu tiên của dãy. Điều này tránh các giá trị trọng điểm không chính xác và đảm bảo tính chính xác ngay cả khi tất cả các số đều âm hoặc giống hệt nhau. 
3. Lặp lại các phần tử còn lại của chuỗi. Đối với mỗi yếu tố, so sánh nó với`mn`và cập nhật`mn`nếu nó nhỏ hơn. Làm tương tự cho`mx`nếu nó lớn hơn. Điều này duy trì cực trị toàn cầu chính xác ở mỗi bước. 
4. Sau khi xử lý tất cả các phần tử, hãy tính phạm vi như`mx - mn`. Điều này hoạt động vì`mx`Và`mn`bây giờ đại diện cho mức tối đa và tối thiểu thực sự của chuỗi đầy đủ. 
5. Nhân phạm vi với`n`để có được “giá trị thú vị” cuối cùng và xuất nó. 

### Tại sao nó hoạt động 

Tại mỗi điểm trong vòng lặp,`mn`Và`mx`chính xác là mức tối thiểu và tối đa của tiền tố được xử lý cho đến nay. Bất biến này đúng vì mỗi phần tử mới sẽ bị bỏ qua hoặc thay thế một trong số chúng nếu nó mở rộng ranh giới. Sau phần tử cuối cùng, tiền tố là toàn bộ mảng, vì vậy`mn`Và`mx`là cực trị toàn cầu thực sự. Vì phạm vi chỉ phụ thuộc vào các cực trị này và không phụ thuộc vào thứ tự nên phép tính cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n_and_rest = sys.stdin.read().strip().split()
n = int(n_and_rest[0])
arr = list(map(int, n_and_rest[1:]))

mn = arr[0]
mx = arr[0]

for x in arr[1:]:
    if x < mn:
        mn = x
    if x > mx:
        mx = x

answer = (mx - mn) * n
print(answer)
```Giải pháp đọc tất cả đầu vào cùng một lúc để tránh phí tổn do các lệnh gọi đầu vào lặp lại. Điều này không bắt buộc phải thực hiện đối với n tối đa 10.000 nhưng vẫn giữ cho việc triển khai rõ ràng và mạnh mẽ. 

Việc khởi tạo của`mn`Và`mx`từ yếu tố đầu tiên là rất quan trọng. Sử dụng trình giữ chỗ như`float('inf')`hoạt động, nhưng việc neo trực tiếp vào mảng sẽ tránh các lỗi biên và giữ cho các phép so sánh nhất quán giữa các loại số nguyên. 

Phép nhân cuối cùng được thực hiện sau khi tính toán phạm vi, đảm bảo rằng các giá trị trung gian không ảnh hưởng đến độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n = 3`, mảng =`[-1, 2, 4]`| Bước | Giá trị hiện tại | mn | mx | 
| --- | --- | --- | --- | 
| Bắt đầu | -1 | -1 | -1 | 
| Quy trình 2 | 2 | -1 | 2 | 
| Quy trình 4 | 4 | -1 | 4 | 

Tính toán cuối cùng: phạm vi = 4 - (-1) = 5, đáp án = 5 * 3 = 15 

Dấu vết này cho thấy cực trị đang chạy phát triển như thế nào và các giá trị âm ảnh hưởng chính xác đến phạm vi như thế nào. 

### Ví dụ 2 

đầu vào:`n = 5`, mảng =`[7, 7, 7, 7, 7]`| Bước | Giá trị hiện tại | mn | mx | 
| --- | --- | --- | --- | 
| Bắt đầu | 7 | 7 | 7 | 
| Tất cả những người khác | 7 | 7 | 7 | 

Tính toán cuối cùng: phạm vi = 0, câu trả lời = 0 

Điều này thể hiện trường hợp suy biến trong đó tất cả các giá trị đều bằng nhau và đảm bảo thuật toán thu gọn chính xác về 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần để duy trì min và max | 
| Không gian | O(1) | Chỉ có hai biến được sử dụng bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 10.000 phần tử và một lần quét tuyến tính duy nhất với công việc liên tục trên mỗi phần tử là thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi nên không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from builtins import input as _input
    # inline solution
    data = inp.strip().split()
    n = int(data[0])
    arr = list(map(int, data[1:]))

    mn = arr[0]
    mx = arr[0]

    for x in arr[1:]:
        if x < mn:
            mn = x
        if x > mx:
            mx = x

    return str((mx - mn) * n)

assert run("3 -1 2 4") == "15"

# single element
assert run("1 42") == "0"

# all equal
assert run("5 7 7 7 7 7") == "0"

# strictly decreasing
assert run("4 10 5 0 -5") == str((10 - (-5)) * 4)

# already sorted increasing
assert run("4 1 2 3 4") == str((4 - 1) * 4)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 42`|`0`| Vỏ cạnh một phần tử | 
|`5 7 7 7 7 7`|`0`| Chuỗi không đổi | 
|`4 10 5 0 -5`|`60`| Xử lý tối thiểu tiêu cực | 
|`4 1 2 3 4`|`12`| Trình tự tăng bình thường | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`1 100000`, thuật toán khởi tạo cả hai`mn`Và`mx`ĐẾN`100000`. Không có cập nhật xảy ra trong quá trình lặp. Phạm vi trở thành`0`, và nhân với`n`vẫn mang lại lợi nhuận`0`, khớp với định nghĩa rằng một giá trị có mức chênh lệch bằng 0. 

Đối với một chuỗi tiêu cực hoàn toàn như`3 -5 -2 -10`, mức chạy tối thiểu trở thành`-10`và cực đại trở thành`-2`. Phạm vi là`8`, và câu trả lời là`24`. Thuật toán tránh được mọi giả định rằng các giá trị phải dương một cách chính xác vì tất cả các so sánh hoàn toàn mang tính chất quan hệ. 

Đối với một chuỗi cực đoan hỗn hợp như`2 -100000 100000`,`mn`trở thành`-100000`Và`mx`trở thành`100000`, sản xuất hàng loạt`200000`. Phép nhân với`n`cho`400000`và điều này xác nhận rằng không xảy ra sự cố tràn hoặc xử lý dấu vì tất cả các phép toán vẫn nằm trong số học số nguyên chính xác tùy ý của Python.
