---
title: "CF 106136A - Ngõ Vàng"
description: "Chúng tôi được cấp một nhóm ICPC nhỏ và một bản ghi số huy chương mà đội đó giành được trong một cuộc thi. Mỗi huy chương vàng, bạc và đồng tương ứng với tổng giải thưởng cố định: vàng trị giá 7500 nhân dân tệ mỗi đội, bạc là 3000 nhân dân tệ mỗi đội và đồng là 1500 nhân dân tệ mỗi đội."
date: "2026-06-20T02:15:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106136
codeforces_index: "A"
codeforces_contest_name: "East China University of Science and Technology Programming Contest 2025"
rating: 0
weight: 106136
solve_time_s: 41
verified: true
draft: false
---

[CF 106136A - Con hẻm vàng](https://codeforces.com/problemset/problem/106136/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một nhóm ICPC nhỏ và một bản ghi số huy chương mà đội đó giành được trong một cuộc thi. Mỗi huy chương vàng, bạc và đồng tương ứng với tổng giải thưởng cố định: vàng trị giá 7500 nhân dân tệ mỗi đội, bạc là 3000 nhân dân tệ mỗi đội và đồng là 1500 nhân dân tệ mỗi đội. Đội có thể có tối đa ba thành viên và tổng số tiền thưởng mà đội kiếm được sẽ được chia đều cho tất cả các thành viên. 

Nhiệm vụ là tính số tiền mà một thành viên cụ thể nhận được, dựa trên quy mô đội và số lượng của từng loại huy chương. 

Dữ liệu đầu vào chứa bốn số nguyên: quy mô đội, tiếp theo là số huy chương vàng, bạc và đồng. Đầu ra là một số nguyên duy nhất biểu thị phần chia của một thành viên trong tổng số tiền thưởng. 

Những hạn chế là cực kỳ nhỏ. Quy mô nhóm tối đa là 3 và tổng số huy chương nhiều nhất là 7. Điều này ngay lập tức loại trừ mọi nhu cầu về kỹ thuật tối ưu hóa hoặc thậm chí là cấu trúc dữ liệu cẩn thận. Mọi phép toán đều là số học có thời gian không đổi. 

Vì tất cả các giá trị đều là số nguyên và cách chia đều chính xác theo cách xây dựng (tổng giải thưởng luôn chia hết cho quy mô đội theo cách hiểu dự kiến), nên cạm bẫy chính chỉ đơn giản là thực hiện phép tính số học một cách chính xác mà không làm mất độ chính xác hoặc nhầm lẫn thứ tự chia. 

Một vài trường hợp khó khăn vẫn đáng được quan tâm. 

Trường hợp một bên là khi không có huy chương nào cả. Ví dụ, đầu vào`2 0 0 0`nên nhường`0`, vì tổng giải thưởng bằng không. 

Một cái khác là quy mô nhóm nhỏ nhất có thể. Ví dụ,`1 1 0 0`nên trả lại toàn bộ giải thưởng vàng là 7500, vì không có gì được chia. 

Thứ ba là giải thưởng không chia đều nếu tính sai do chia số nguyên sớm. Ví dụ: nếu một người chia nhầm từng giải thưởng huy chương cho mỗi người trước khi tính tổng, lỗi dấu phẩy động hoặc lỗi cắt ngắn có thể xuất hiện. Cách làm đúng là tính tổng giải thưởng trước rồi chia một lần. 

## Phương pháp tiếp cận 

Ở đây, việc giải thích bạo lực gần như không cần thiết, nhưng chúng ta vẫn có thể mô tả nó như mô phỏng từng sự kiện huy chương và tích lũy giải thưởng cho mỗi thành viên trong đội. Đối với mỗi huy chương vàng, chúng tôi sẽ cộng thêm 7500 vào tổng số điểm đang chạy, mỗi huy chương bạc cộng thêm 3000 và mỗi huy chương đồng cộng thêm 1500, sau đó cuối cùng chia cho quy mô đội. 

Ngay cả khi chúng tôi nghĩ sai về việc phân phát từng phần thưởng huy chương cho các thành viên ngay lập tức, thì số lượng hoạt động trong trường hợp xấu nhất vẫn bị giới hạn bởi số lượng huy chương, nhiều nhất là 7. Vì vậy, ngay cả việc mô phỏng theo nghĩa đen của mọi sự kiện huy chương cũng là chuyện nhỏ. 

Quan sát quan trọng là vấn đề hoàn toàn có tính chất cộng tính và tuyến tính. Thứ tự xử lý các huy chương không quan trọng và không có sự tương tác giữa các huy chương. Điều này làm giảm mọi thứ thành việc tính tổng có trọng số và chia một lần ở cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng vũ phu | O(g + s + b) | O(1) | Đã chấp nhận | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên biểu thị quy mô đội và số huy chương vàng, bạc và đồng. Điều này thiết lập tất cả các đầu vào cần thiết cho việc tính toán trong một bước duy nhất. 
2. Tính tổng giải thưởng mà đội giành được dưới dạng tổng có trọng số: mỗi huy chương vàng đóng góp 7500, mỗi huy chương bạc đóng góp 3000 và mỗi huy chương đồng đóng góp 1500. Bước này thu gọn toàn bộ lịch sử cuộc thi thành một giá trị vô hướng duy nhất, giá trị này hợp lệ vì mỗi huy chương đóng góp độc lập. 
3. Chia tổng giải thưởng cho quy mô đội. Vì đội chia đều số tiền thắng nên điều này tạo ra số tiền mà mỗi thành viên nhận được. 
4. Xuất kết quả dưới dạng số nguyên. 

### Tại sao nó hoạt động 

Hệ thống giải thưởng là tuyến tính đối với số lượng huy chương. Mỗi huy chương đóng góp một số tiền cố định độc lập vào tổng phần thưởng và thao tác chia tách được áp dụng một lần cho số tiền cuối cùng. Vì phép chia được áp dụng sau khi tính tổng nên tính phân phối đảm bảo kết quả tương đương với việc phân chia đều từng phần thưởng huy chương cho các thành viên rồi tính tổng. Điều này đảm bảo rằng không có hiệu ứng thứ tự hoặc nhóm nào có thể thay đổi câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, g, s, b = map(int, input().split())

total = g * 7500 + s * 3000 + b * 1500
print(total // n)
```Giải pháp đọc đầu vào trong một dòng và ngay lập tức giải nén bốn số nguyên. Tổng giải thưởng được tính bằng cách nhân trực tiếp số lượng với giá trị tương ứng của chúng, điều này tránh bất kỳ vòng lặp nào. 

Phép chia được thực hiện ở phần cuối bằng cách sử dụng phép chia số nguyên. Điều này là an toàn vì tất cả các giá trị giải thưởng đều là bội số của 500 và việc chia nhằm mục đích chính xác theo định nghĩa của bài toán. sử dụng`//`đảm bảo chúng tôi duy trì số học số nguyên, tránh các vấn đề về dấu phẩy động. 

Một sai lầm phổ biến là chia từng khoản đóng góp huy chương riêng biệt trước khi tính tổng. Điều đó vẫn có tác dụng về mặt toán học ở đây, nhưng nó gây ra rủi ro không cần thiết về lỗi làm tròn trong các bài toán tổng quát hơn và che khuất cấu trúc dự định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 2 1 2`Chúng tôi tính toán tổng giải thưởng từng bước. 

| Bước | Vàng | Bạc | Đồng | Tổng số tính toán | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 2 | - | 0 | 
| Sau vàng | 2 | 1 | 2 | 2 × 7500 | 15000 | 
| Sau bạc | 2 | 1 | 2 | + 1×3000 | 18000 | 
| Sau đồng | 2 | 1 | 2 | + 2×1500 | 21000 | 

Bây giờ chia cho quy mô đội 3: 21000 3 = 7000. 

Dấu vết này xác nhận rằng việc tổng hợp trước khi phân chia phù hợp với việc chia đều tổng thu nhập như dự kiến. 

### Ví dụ 2 

đầu vào:`1 0 0 4`| Bước | Vàng | Bạc | Đồng | Tổng số tính toán | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 4 | - | 0 | 
| Sau đồng | 0 | 0 | 4 | 4 × 1500 | 6000 | 

Chia cho quy mô đội 1: 6000 1 = 6000. 

Trường hợp này xác nhận rằng công thức xử lý các nhóm một thành viên một cách tự nhiên mà không cần bất kỳ logic đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi bất kể đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc là cực kỳ nhỏ, do đó, ngay cả việc triển khai dài dòng hơn cũng sẽ phù hợp một cách thoải mái trong giới hạn. Cách tiếp cận công thức trực tiếp là tối ưu và thực hiện trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, g, s, b = map(int, input().split())
    total = g * 7500 + s * 3000 + b * 1500
    return str(total // n)

# provided samples
assert run("3 2 1 2") == "7000", "sample 1"
assert run("1 0 0 4") == "6000", "sample 2"

# custom cases
assert run("1 0 0 0") == "0", "no medals"
assert run("3 0 0 0") == "0", "team with no reward"
assert run("2 1 1 1") == str((7500 + 3000 + 1500)//2), "mixed medals"
assert run("3 7 0 0") == str((7*7500)//3), "max gold case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 0 | 0 | trường hợp không có phần thưởng | 
| 3 0 0 0 | 0 | không có huy chương khi chia đội | 
| 2 1 1 1 | 6000 | tổng hợp huy chương hỗn hợp | 
| 3 7 0 0 | 17500 | ranh giới vàng tối đa | 

## Vỏ cạnh 

Đối với đầu vào`3 0 0 0`, thuật toán tính tổng giải thưởng bằng 0 và chia cho 3, tạo ra 0. Điều này xác nhận rằng bước chia không đưa ra các tạo phẩm khi tử số bằng 0. 

Vì`1 0 0 4`, phép tính mang lại 6000 trước khi chia. Vì quy mô nhóm là một nên phép chia không thay đổi giá trị, xác nhận tính chính xác đối với quy mô nhóm tối thiểu. 

Vì`3 7 0 0`, tổng giải thưởng là`7 × 7500 = 52500`, và kết quả phép chia`17500`. Điều này chứng tỏ rằng thuật toán xử lý số lượng huy chương tối đa mà không lo ngại tràn trong Python và áp dụng chính xác phép chia số nguyên ở cuối.
