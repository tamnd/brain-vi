---
title: "CF 105790G - Gargantua"
description: "Tình huống mô tả hai phi hành gia và sự chênh lệch thời gian tương đối tính do hệ thống lỗ đen gây ra. Một phi hành gia, Leo, vẫn ở lại Trái đất trong khi người còn lại, Ema, du hành đến một hành tinh xa xôi, nơi thời gian trôi chậm hơn."
date: "2026-06-25T23:32:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "G"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 44
verified: true
draft: false
---

[CF 105790G - Gargantua](https://codeforces.com/problemset/problem/105790/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tình huống mô tả hai phi hành gia và sự chênh lệch thời gian tương đối tính do hệ thống lỗ đen gây ra. Một phi hành gia, Leo, vẫn ở lại Trái đất trong khi người còn lại, Ema, du hành đến một hành tinh xa xôi, nơi thời gian trôi chậm hơn. Chi tiết quan trọng là thời gian trên hành tinh này không khớp tuyến tính với thời gian trên Trái đất mà bị kéo dài bởi một hệ số cố định. 

Chúng ta được cho ba số nguyên. Đầu tiên là tuổi ban đầu của Leo tại thời điểm Ema rời Trái đất. Thứ hai là hệ số giãn nở thời gian cho biết thời gian trôi qua trên hành tinh chậm hơn bao nhiêu so với Trái đất. Thứ ba là Ema sống trên hành tinh này bao nhiêu năm tính theo đồng hồ của hành tinh đó. 

Nhiệm vụ là xác định tuổi của Leo khi Ema quay trở lại Trái đất, điều này phụ thuộc hoàn toàn vào thời gian Trái đất trôi qua trong chuyến đi của Ema. 

Bước diễn giải quan trọng là chuyển đổi thời gian trên hành tinh này thành thời gian trên Trái đất. Nếu một năm trên hành tinh tương ứng với X năm trên Trái đất, thì khoảng thời gian Y năm hành tinh tương ứng với X × Y năm Trái đất. Sư Tử già đi bình thường trong toàn bộ khoảng thời gian trên Trái đất này. 

Giới hạn đầu vào cực kỳ nhỏ, với tất cả các giá trị được giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa hoặc cấu trúc dữ liệu nâng cao. Ngay cả một phép tính số học trực tiếp cũng đủ trong thời gian không đổi. 

Một trường hợp lỗi nhỏ xuất hiện khi bước nhân bị xử lý sai. Ví dụ: nếu X và Y được coi là dấu phẩy động hoặc nếu có thể xảy ra tràn số nguyên trong các ràng buộc lớn hơn thì kết quả có thể không chính xác. Tuy nhiên, ở đây các giá trị đủ nhỏ để số học số nguyên tiêu chuẩn được an toàn. 

Một sai lầm tiềm ẩn khác là hiểu sai hướng giãn nở thời gian. Cụm từ “thời gian trôi chậm hơn X lần” có thể bị hiểu sai là phép chia thay vì phép nhân. Nếu ai đó tính Y / X thay vì X × Y, họ sẽ nhận được kết quả nhỏ hơn và không chính xác ngay cả trên các dữ liệu đầu vào đơn giản như A = 20, X = 3, Y = 4, trong đó đóng góp chính xác của thời gian Trái đất là 12 chứ không phải 1. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ về quá trình này sẽ mô phỏng thời gian theo từng năm. Người ta có thể tưởng tượng việc nâng cao thời gian lưu trú của Ema theo từng hành tinh một và chuyển đổi từng thời gian trên Trái đất bằng cách nhân với X, sau đó tăng tuổi của Leo cho phù hợp. Điều này hoạt động hợp lý nhưng tạo ra sự lặp lại không cần thiết qua các bước Y. Mỗi bước thực hiện công việc không đổi, do đó độ phức tạp tổng cộng là O(Y), ở đây vẫn còn tầm thường nhưng trở nên kém hiệu quả về mặt khái niệm. 

Quan sát quan trọng là sự chuyển đổi từ thời gian hành tinh sang thời gian Trái đất là tuyến tính và đồng nhất. Mỗi đơn vị thời gian trên hành tinh sẽ giãn ra thành chính xác X đơn vị trên Trái đất. Do tỷ lệ cố định này, toàn bộ thời lượng có thể được chuyển đổi thành một lần nhân thay vì tích lũy nhiều lần. 

Điều này biến bài toán thành một biểu thức số học trực tiếp: Tuổi cuối cùng của Leo bằng tuổi ban đầu cộng với tổng thời gian trên Trái đất trôi qua trong sứ mệnh của Ema, đó là X × Y. 

Không có sự phụ thuộc giữa các trạng thái trung gian nên không cần mô phỏng hoặc theo dõi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(Y) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tính toán trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tuổi ban đầu A, hệ số giãn nở thời gian X và khoảng thời gian tồn tại của hành tinh Y. Đây là các giá trị vô hướng độc lập mô tả trạng thái hệ thống. 
2. Tính tổng thời gian Trái đất trôi qua trong sứ mệnh của Ema bằng cách nhân X và Y. Sự chuyển đổi này phản ánh mối quan hệ vật lý giữa hai thang thời gian. 
3. Thêm thời gian Trái đất này vào tuổi ban đầu của Sư Tử A. Sư Tử già đi với tốc độ tương đương với thời gian Trái đất, vì vậy mỗi năm Trái đất sẽ tăng tuổi của anh ấy lên một. 
4. In ra giá trị kết quả là tuổi của Leo tại thời điểm Ema trở về. 

## Tại sao nó hoạt động

Tính đúng đắn phụ thuộc vào thực tế là độ giãn nở thời gian không đổi trong toàn bộ khoảng thời gian. Mỗi năm hành tinh đóng góp chính xác X năm Trái đất, do đó, việc ánh xạ từ thời gian hành tinh đến thời gian Trái đất là một tỷ lệ tuyến tính. Vì sự già đi của Leo chỉ phụ thuộc vào giờ Trái đất chứ không phụ thuộc vào giờ địa phương của Ema nên kết quả hoàn toàn được xác định bởi A + X × Y. Không có tương tác trung gian hoặc thay đổi trạng thái nào ảnh hưởng đến kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = int(input().strip())
    x = int(input().strip())
    y = int(input().strip())
    print(a + x * y)

if __name__ == "__main__":
    solve()
```Giải pháp đọc ba số nguyên từ các dòng riêng biệt, như được chỉ định. phép nhân`x * y`được thực hiện trực tiếp mà không cần bất kỳ vòng lặp hoặc chuyển đổi nào. Kết quả được thêm vào`a`, đại diện cho tuổi ban đầu của Leo. 

Một lỗi triển khai phổ biến là cố đọc tất cả các giá trị từ một dòng, nhưng định dạng đầu vào đặt rõ ràng từng giá trị trên một dòng riêng, vì vậy mỗi lệnh gọi đến`input()`phải được xử lý riêng. 

Một chi tiết khác là đảm bảo phép nhân xảy ra trước phép cộng, mặc dù độ ưu tiên của toán tử tạo nên biểu thức`a + x * y`tự nhiên đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
20
3
4
```Chúng tôi theo dõi tính toán: 

| Bước | A | X | Y | Giờ Trái Đất (X×Y) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 20 | 3 | 4 | - | - | 
| Tính giờ Trái đất | 20 | 3 | 4 | 12 | - | 
| Thêm vào tuổi | 20 | 3 | 4 | 12 | 32 | 

Điều này chứng tỏ rằng Leo trải qua thêm 12 năm Trái đất trong sứ mệnh của Ema, dẫn đến tuổi cuối cùng là 32. 

### Ví dụ 2 

đầu vào:```
31
17
3
```| Bước | A | X | Y | Giờ Trái Đất (X×Y) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 31 | 17 | 3 | - | - | 
| Tính giờ Trái đất | 31 | 17 | 3 | 51 | - | 
| Thêm vào tuổi | 31 | 17 | 3 | 51 | 82 | 

Ở đây, sứ mệnh tương ứng với 51 năm Trái đất nên tuổi của Leo cũng tăng theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện bất kể giá trị đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc đảm bảo tất cả các giá trị đều là số nguyên nhỏ, do đó việc tính toán diễn ra tức thời và nằm trong giới hạn đối với bất kỳ môi trường thời gian chạy tiêu chuẩn nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = int(sys.stdin.readline().strip())
    x = int(sys.stdin.readline().strip())
    y = int(sys.stdin.readline().strip())
    return str(a + x * y)

# provided samples
assert run("20\n3\n4\n") == "32", "sample 1"
assert run("31\n17\n3\n") == "82", "sample 2"

# custom cases
assert run("0\n0\n10\n") == "0", "zero factor eliminates time growth"
assert run("10\n1\n0\n") == "10", "no travel time means no change"
assert run("5\n2\n10\n") == "25", "basic scaling check"
assert run("100\n100\n1\n") == "200", "boundary multiplication case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0,0,10 | 0 | Hệ số giãn nở bằng không | 
| 10,1,0 | 10 | Thời lượng bằng không | 
| 5,2,10 | 25 | Chia tỷ lệ tuyến tính tiêu chuẩn | 
| 100.100,1 | 200 | Phép nhân giới hạn trên | 

## Vỏ cạnh 

Trường hợp góc trực tiếp là khi hệ số giãn nở thời gian bằng 0. Trong trường hợp đó, không có thời gian trên Trái đất trôi qua bất kể Ema ở lại hành tinh này bao lâu. Đối với đầu vào`A=10, X=0, Y=100`, kết quả tính toán`10 + 0 = 10`, nghĩa là Leo không già đi trong thời gian thực hiện nhiệm vụ. 

Một trường hợp khác là khi Ema không dành thời gian trên hành tinh này. Vì`A=10, X=5, Y=0`, phần đóng góp theo thời gian trên Trái đất trở thành 0 và tuổi của Sư Tử không thay đổi ở mức 10. 

Trường hợp ranh giới cuối cùng là khi tất cả các giá trị đều tối đa trong các ràng buộc, chẳng hạn như`A=100, X=100, Y=100`. Phép nhân mang lại 10000 năm Trái đất, dẫn đến tuổi cuối cùng là 10100, vẫn dễ dàng khớp với giới hạn số nguyên và chứng minh rằng không cần có biện pháp phòng ngừa tràn trong cài đặt bài toán này.
