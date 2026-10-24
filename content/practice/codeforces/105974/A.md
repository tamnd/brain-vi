---
title: "CF 105974A - Chuỗi Xor riêng biệt"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta xem xét tất cả các dãy con có thể được hình thành bằng cách chọn bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự. Đối với mỗi chuỗi con được chọn, chúng tôi tính toán XOR theo bit của tất cả các phần tử của nó."
date: "2026-06-25T13:33:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105974
codeforces_index: "A"
codeforces_contest_name: "Introductory Problems: XOR Basis"
rating: 0
weight: 105974
solve_time_s: 48
verified: true
draft: false
---

[CF 105974A - Chuỗi con Xor riêng biệt](https://codeforces.com/problemset/problem/105974/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta xem xét tất cả các dãy con có thể được hình thành bằng cách chọn bất kỳ tập hợp con nào của các phần tử trong khi vẫn giữ nguyên thứ tự. Đối với mỗi chuỗi con được chọn, chúng tôi tính toán XOR theo bit của tất cả các phần tử của nó. Nhiệm vụ là xác định có thể tạo ra bao nhiêu giá trị XOR riêng biệt trên tất cả các chuỗi con. 

Mặc dù các chuỗi con vẫn giữ nguyên trật tự, nhưng bản thân thao tác XOR lại bỏ qua thứ tự, vì vậy điều quan trọng là phần tử nào được chọn thay vì chúng xuất hiện ở đâu. Hai chuỗi con khác nhau vẫn có thể tạo ra cùng một giá trị XOR và chúng tôi được yêu cầu đếm xem có thể có bao nhiêu kết quả duy nhất. 

Nếu độ dài mảng lên tới khoảng 10^5 thì bất kỳ cách tiếp cận nào liệt kê trực tiếp các chuỗi con đều không thể thực hiện được. Số lượng các chuỗi con là 2^n, đã vượt quá mọi tính toán khả thi khi n thậm chí còn lớn vừa phải. Điều này ngay lập tức loại trừ bất kỳ chiến lược liệt kê hàm mũ nào và đẩy chúng ta tới việc biểu diễn tập hợp tất cả các XOR tập hợp con ở dạng nén. 

Một trường hợp lỗi phổ biến xuất phát từ việc cố gắng “theo dõi tất cả XOR” trong một tập hợp theo từng bước. Ví dụ: nếu mảng là [1, 2, 3], người ta có thể mô phỏng việc thêm các phần tử và cập nhật một tập hợp XOR có thể truy cập. Điều này hoạt động với n nhỏ nhưng tăng gấp đôi không gian trạng thái ở mỗi bước trong trường hợp xấu nhất. Với n = 30, điều này đã trở thành đường biên và với n = 10^5 thì điều đó là không thể. 

Một vấn đề tế nhị khác là các yếu tố trùng lặp. Ví dụ: [5, 5] tạo ra các XOR chuỗi con {0, 5}. Việc triển khai đơn giản xử lý các lần xuất hiện một cách độc lập mà không có cấu trúc có thể đếm quá mức hoặc bỏ sót các lần hủy trừ khi nó xử lý chính xác đại số XOR một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra mọi chuỗi con, tính toán XOR của nó và chèn nó vào một tập hợp. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, số lượng chuỗi con là 2^n và mỗi phép tính XOR có chi phí lên tới O(n), do đó tổng công việc sẽ vào khoảng O(n · 2^n). Điều này trở nên không khả thi đối với n vào khoảng 25. 

Quan sát quan trọng là XOR trên các tập hợp con tạo thành cấu trúc tuyến tính trên trường nhị phân. Mỗi số có thể được hiểu là một vectơ bit và XOR tương ứng với vectơ cộng mod 2. Tập hợp tất cả các XOR tập hợp con chính xác là khoảng tuyến tính của các vectơ đã cho. Thay vì liệt kê các tổ hợp, chúng ta chỉ cần hiểu chiều của khoảng này. 

Điều này dẫn đến khái niệm cơ sở tuyến tính (còn gọi là cơ sở XOR). Chúng tôi duy trì một tập hợp các vectơ độc lập sao cho bất kỳ phần tử mảng nào cũng có thể được biểu diễn dưới dạng XOR của một số phần tử cơ bản. Mỗi lần chèn một số mới, chúng tôi cố gắng giảm số đó bằng cách sử dụng các vectơ cơ sở đã lưu trước đó. Nếu không thể giảm về 0 thì nó sẽ trở thành một hướng độc lập mới. 

Nếu cơ sở cuối cùng có kích thước k thì mỗi tập con XOR tương ứng với việc chọn có bao gồm từng vectơ cơ sở hay không một cách độc lập. Điều đó tạo ra chính xác 2^k giá trị XOR riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(2^n) | Quá chậm | 
| Cơ sở tuyến tính | O(n log A) | O(1) đến O(log A) | Đã chấp nhận | 

Ở đây A là giá trị lớn nhất trong mảng. 

## Hướng dẫn thuật toán

1. Khởi tạo cấu trúc cơ sở XOR trống trong đó mỗi vị trí tương ứng với một bit từ cao đến thấp. Cấu trúc này sẽ lưu trữ tối đa một vectơ cho mỗi vị trí bit. 
2. Lặp qua từng số trong mảng. Đối với số hiện tại, hãy cố gắng giảm nó bằng cách sử dụng cơ sở hiện có. Đối với mỗi bit từ cao nhất đến thấp nhất, nếu bit đó được đặt và chúng ta đã có vectơ cơ sở cho bit đó, hãy XOR số có vectơ cơ sở đó để loại bỏ bit đứng đầu. Bước này dần dần đơn giản hóa số trong khi vẫn duy trì tính đại diện của nó trong khoảng. 
3. Sau khi rút gọn, nếu số trở về 0 nghĩa là nó có thể được hình thành từ các phần tử cơ sở hiện có nên không làm tăng tính chiều của không gian. 
4. Nếu số khác 0, hãy tìm bit được đặt cao nhất của nó và chèn nó vào cơ sở tại vị trí đó. Điều này mở rộng tập hợp các vectơ độc lập. 
5. Sau khi xử lý tất cả các số, đếm xem có bao nhiêu vectơ cơ sở được chèn vào. Đặt số này là k. 
6. Câu trả lời là 2^k, đại diện cho tất cả các kết hợp XOR có thể có của các hướng độc lập. 

### Tại sao nó hoạt động 

Cơ sở duy trì một tập hợp các vectơ XOR độc lập tuyến tính. Mỗi vectơ được chèn đều nằm trong khoảng của các vectơ hiện có hoặc tăng kích thước lên đúng một. Vì XOR là phép cộng trong không gian vectơ trên GF(2), nên mỗi tập con XOR tương ứng duy nhất với một lựa chọn vectơ cơ sở. Do đó, số giá trị XOR có thể truy cập chính xác là số phần tử trong khoảng, được nâng lên 2 theo chiều của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    B = [0] * 60  # basis for bits up to 2^59
    rank = 0

    for x in arr:
        v = x
        for b in range(59, -1, -1):
            if (v >> b) & 1:
                if B[b]:
                    v ^= B[b]
                else:
                    B[b] = v
                    rank += 1
                    break

    # number of distinct XOR subsets
    print(1 << rank)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một mảng có kích thước cố định`B`trong đó mỗi mục lưu trữ một vectơ đại diện có bit cao nhất ở vị trí đó. Điều này đảm bảo rằng mỗi lần chèn mới sẽ tự hủy bỏ hoặc đưa ra một hướng độc lập mới. Biến`rank`theo dõi xem có bao nhiêu hướng độc lập như vậy đã được phát hiện. 

Hoạt động ca cuối cùng`1 << rank`tính toán thứ hạng 2^một cách hiệu quả. Vì thứ hạng tối đa là 60 đối với số nguyên 64 bit tiêu chuẩn nên điều này là an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi xử lý từng số và duy trì các vectơ cơ sở. 

| Bước | x | Giảm x | Thay đổi cơ bản | Xếp hạng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | thêm 1 | 1 | 
| 2 | 2 | 2 | thêm 2 | 2 | 
| 3 | 3 | 0 | không thay đổi | 2 | 

Giá trị thứ ba giảm về 0 vì 3 = 1 XOR 2. Điều này xác nhận rằng nó không làm tăng số chiều. Thứ hạng cuối cùng là 2, vì vậy số lượng XOR riêng biệt là 2^2 = 4. 

### Ví dụ 2 

đầu vào:```
4
5 5 5 5
```| Bước | x | Giảm x | Thay đổi cơ bản | Xếp hạng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | thêm 5 | 1 | 
| 2 | 5 | 0 | không thay đổi | 1 | 
| 3 | 5 | 0 | không thay đổi | 1 | 
| 4 | 5 | 0 | không thay đổi | 1 | 

Tất cả các phần tử lặp lại sẽ giảm về 0 sau lần chèn đầu tiên. Chỉ tồn tại một vectơ độc lập nên đáp án là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được giảm tối đa ở vị trí 60 bit | 
| Không gian | O(1) | Mảng cơ sở có kích thước cố định có độ dài không đổi | 

Các ràng buộc cho phép tối đa 10^5 phần tử và mỗi phần tử được xử lý trong tối đa 60 bước, cung cấp khoảng 6 triệu thao tác bit, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    B = [0] * 60
    rank = 0

    for x in arr:
        v = x
        for b in range(59, -1, -1):
            if (v >> b) & 1:
                if B[b]:
                    v ^= B[b]
                else:
                    B[b] = v
                    rank += 1
                    break

    return str(1 << rank)

# small cases
assert run("1\n0") == "1"
assert run("2\n1 1") == "2"
assert run("3\n1 2 3") == "4"

# all equal
assert run("5\n7 7 7 7 7") == "2"

# powers of two
assert run("3\n1 2 4") == "8"

# mixed
assert run("4\n1 2 4 7") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử số 0 | 1 | trường hợp nhịp trống | 
| trùng lặp | 2 | xử lý hủy bỏ | 
| quyền lực độc lập | 8 | tăng trưởng thứ hạng đầy đủ | 
| cấu trúc XOR hỗn hợp | 8 | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Một đầu vào phần tử 0 duy nhất, chẳng hạn như [0] sẽ tạo ra chính xác một giá trị XOR riêng biệt, bởi vì các chuỗi con duy nhất đều trống và [0], cả hai đều mang lại kết quả bằng 0. Thuật toán coi số 0 là có thể giảm ngay lập tức và không tăng thứ hạng, do đó kết quả là 2^0 = 1. 

Một mảng trong đó mọi phần tử đều giống hệt nhau, ví dụ [9, 9, 9], cũng chỉ tạo ra hai XOR riêng biệt: 0 và chính giá trị đó. Lần chèn đầu tiên thiết lập một vectơ cơ sở và mọi giá trị giống hệt tiếp theo sẽ giảm về 0 theo cơ sở đó, giữ nguyên thứ hạng 1 và tạo ra 2^1 = 2. 

Cấu trúc xếp hạng đầy đủ như [1, 2, 4, 8] đảm bảo rằng mọi số đều trở thành một vectơ cơ sở độc lập. Mỗi lần chèn sẽ tăng thứ hạng lên một và kết quả cuối cùng 2^4 = 16 khớp với thực tế là tất cả các tập hợp con XOR đều khác biệt.
