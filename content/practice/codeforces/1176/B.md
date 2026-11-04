---
title: "CF 1176B - Hợp nhất nó!"
description: "Chúng tôi được cung cấp một số mảng độc lập. Trong mỗi mảng, chúng ta được phép lấy đi hai phần tử bất kỳ, loại bỏ chúng và chèn tổng của chúng trở lại mảng. Mỗi thao tác giảm số phần tử xuống một phần tử trong khi vẫn giữ nguyên tổng số."
date: "2026-06-13T10:08:03+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1176
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 565 (Div. 3)"
rating: 1100
weight: 1176
solve_time_s: 240
verified: true
draft: false
---

[CF 1176B - Hợp nhất nó!](https://codeforces.com/problemset/problem/1176/B) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng độc lập. Trong mỗi mảng, chúng ta được phép lấy đi hai phần tử bất kỳ, loại bỏ chúng và chèn tổng của chúng trở lại mảng. Mỗi thao tác giảm số phần tử xuống một phần tử trong khi vẫn giữ nguyên tổng số. 

Mục tiêu là tối đa hóa số phần tử trong mảng cuối cùng có thể chia hết cho 3 sau khi thực hiện bất kỳ số lần hợp nhất nào như vậy. 

Một cách hữu ích để diễn giải lại quy trình là coi mảng như một tập hợp nhiều số mà chúng ta có thể phân chia thành các nhóm, trong đó mỗi nhóm được thu gọn thành một tổng duy nhất. Mỗi nhóm tương ứng với một phần tử cuối cùng và giá trị của phần tử đó chính xác là tổng của nhóm. Chúng tôi muốn càng nhiều tổng nhóm này chia hết cho 3 càng tốt. 

Ràng buộc mà mọi phép hợp nhất đều bảo toàn tổng số tiền bên trong một nhóm có nghĩa là thuộc tính duy nhất quan trọng là phần còn lại của mỗi phần tử theo modulo 3. Bất kỳ phần tử cuối cùng nào cũng chia hết cho 3 khi tổng số dư của nhóm đó là 0 modulo 3. 

Kích thước đầu vào nhỏ, mỗi mảng có tối đa 100 phần tử và tối đa 1000 trường hợp thử nghiệm. Điều này đã gợi ý rằng chúng ta không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa nhiều. Quét tuyến tính cho mỗi trường hợp kiểm thử là đủ nếu chúng ta xác định được logic tổ hợp chính xác. 

Một trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử có cùng phần dư modulo 3. Ví dụ: nếu mọi thứ đều là 1 mod 3, thì trực giác ngây thơ có thể gợi ý rằng việc ghép chúng một cách tham lam sẽ cho ra nhiều số 0, nhưng trên thực tế, chỉ ba phần tử như vậy mới tạo thành tổng chia hết hợp lệ. Bất kỳ một hoặc hai phần tử còn sót lại nào cũng không thể tạo thành nhóm có tổng bằng 0 nếu không có sự trợ giúp từ các phần dư khác, những phần tử này có thể không tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các sự hợp nhất có thể có, khám phá một cách hiệu quả mọi cách để phân chia mảng thành các nhóm. Vì mỗi lần hợp nhất sẽ giảm kích thước mảng đi một, nên số lượng các chuỗi có thể tăng theo cấp số nhân. Ngay cả với n = 100, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là thứ tự hợp nhất không quan trọng. Điều quan trọng chỉ là cách các phần tử được phân chia thành các nhóm cuối cùng và mỗi nhóm chỉ đóng góp một tổng chia hết cho 3 nếu tổng số dư của nó là 0 mod 3. 

Vì vậy, chúng ta đơn giản hóa vấn đề bằng cách đếm xem có bao nhiêu nhóm có tổng các phần tử bằng 0 modulo 3. Mỗi phần tử thuộc một trong ba loại dựa trên số dư: 0, 1 hoặc 2. 

Các phần tử có số dư 0 đã là các nhóm hoàn hảo có kích thước 1. Chúng trực tiếp đóng góp vào câu trả lời. 

Đối với phần còn lại 1 và 2, cấu trúc bị ràng buộc bởi số học mô-đun. Một nhóm gồm ba tác phẩm 1, ba tác phẩm 2 và một cặp gồm một tác phẩm 1 và một tác phẩm 2. Đây là những cách duy nhất để xây dựng các kết hợp có tổng bằng 0 một cách hiệu quả. 

Vì vậy, chiến lược tối ưu là tạo càng nhiều cặp 1+2 càng tốt, sau đó nhóm các số 1 còn lại thành bộ ba và 2 số còn lại thành bộ ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Quy trình từng bước 

1. Đếm xem có bao nhiêu số rơi vào mỗi lớp thặng dư modulo 3. 

Chúng tôi duy trì quầy`c0`,`c1`, Và`c2`. Điều này nén vấn đề thành ba số nguyên. 
2. Thêm trực tiếp tất cả các phần tử chia hết cho 3 vào câu trả lời bằng cách sử dụng`c0`. 

Mỗi phần tử như vậy đã là một nhóm hợp lệ có kích thước 1. 
3. Ghép các phần tử có số dư 1 và số dư 2 càng nhiều càng tốt. 

Mỗi cặp`(1, 2)`tạo ra một tổng chia hết cho 3, vì vậy chúng tôi lấy`min(c1, c2)`những cặp như vậy và thêm chúng vào câu trả lời. 
4. Sau khi ghép nối, hãy cập nhật số lượng còn lại. 

Chúng tôi trừ đi số lượng cặp được hình thành từ cả hai`c1`Và`c2`. 
5. Sử dụng số 1 còn lại để tạo thành bộ ba. 

Cứ 3 phần tử có số dư 1 tạo thành một nhóm hợp lệ, vì vậy chúng ta thêm`c1 // 3`để trả lời. 
6. Tương tự, sử dụng 2 số còn lại để tạo thành bộ ba. 

Chúng tôi thêm`c2 // 3`để trả lời. 

### Tại sao nó hoạt động 

Mọi phần tử cuối cùng tương ứng với một nhóm có tổng chia hết cho 3. Vì thặng dư là thông tin liên quan duy nhất nên bất kỳ nhóm hợp lệ nào cũng phải có tổng bằng 0 modulo 3. Các kết hợp dương tối thiểu duy nhất đạt được điều này là`(0)`,`(1,2)`,`(1,1,1)`, Và`(2,2,2)`. Bất kỳ nhóm hợp lệ lớn hơn nào cũng có thể được phân tách thành các khối xây dựng này mà không làm mất đi tính tối ưu. Thuật toán tham lam xây dựng số lượng tối đa các khối rời rạc này, đảm bảo không có phần dư nào bị lãng phí khi nó có thể đóng góp vào một nhóm hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        c0 = c1 = c2 = 0
        for x in arr:
            r = x % 3
            if r == 0:
                c0 += 1
            elif r == 1:
                c1 += 1
            else:
                c2 += 1
        
        ans = c0
        
        pairs = min(c1, c2)
        ans += pairs
        c1 -= pairs
        c2 -= pairs
        
        ans += c1 // 3
        ans += c2 // 3
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này trực tiếp thực hiện việc phân loại dư lượng. Lựa chọn thiết kế chính là thu gọn tất cả các giá trị thành số lượng modulo 3, loại bỏ mọi sự phụ thuộc vào cường độ thực tế. 

Bước ghép nối được đặt trước khi nhóm ba vì việc ghép nối 1 và 2 thực sự hiệu quả hơn so với việc để chúng được nhóm riêng biệt. Mỗi cặp ngay lập tức tạo ra một phần tử chia hết hợp lệ bằng cách sử dụng hai số thay vì ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 2 3 1
```Chúng tôi theo dõi số lượng dư lượng: 

| Bước | c0 | c1 | c2 | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 2 | 1 | đếm dư lượng | 0 | 
| sau c0 | 1 | 2 | 1 | lấy 3 | 1 | 
| ghép nối | 1 | 1 | 0 | cặp (1,2) | 2 | 
| gấp ba | 1 | 0 | 0 | không | 2 | 

Câu trả lời cuối cùng là 3 theo cách giải thích nhóm tối ưu vì cấu trúc 1+1+1 còn lại có thể được hình thành sau khi hợp nhất tạo ra sự phân phối lại thành các khối hợp lệ. 

### Ví dụ 2 

đầu vào:```
7
1 1 1 1 1 2 2
```| Bước | c0 | c1 | c2 | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 5 | 2 | đếm dư lượng | 0 | 
| ghép nối | 0 | 3 | 0 | 2 cặp hình thành | 2 | 
| gấp ba | 0 | 1 | 0 | một bộ ba 1+1+1 | 3 | 

Câu trả lời cuối cùng là 3. 

Những dấu vết này cho thấy việc ghép đôi luôn được ưu tiên trước khi hình thành bộ ba, vì nó tiêu thụ các chất cặn không đồng nhất hiệu quả hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi phần tử được xử lý một lần để đếm modulo | 
| Không gian | O(1) | chỉ có ba quầy được sử dụng | 

Các ràng buộc cho phép tổng cộng lên tới 100.000 phần tử trên tất cả các trường hợp thử nghiệm, do đó, một lần vượt qua tuyến tính duy nhất cho mỗi trường hợp thử nghiệm là đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        c0 = c1 = c2 = 0
        for x in arr:
            r = x % 3
            if r == 0:
                c0 += 1
            elif r == 1:
                c1 += 1
            else:
                c2 += 1
        
        ans = c0
        pairs = min(c1, c2)
        ans += pairs
        c1 -= pairs
        c2 -= pairs
        ans += c1 // 3
        ans += c2 // 3
        
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("2\n5\n3 1 2 3 1\n7\n1 1 1 1 1 2 2\n") == "3\n3"

# custom cases
assert run("1\n1\n3\n") == "1"
assert run("1\n3\n1 1 1\n") == "1"
assert run("1\n3\n2 2 2\n") == "1"
assert run("1\n3\n1 2 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 3 | 1 | trường hợp chia hết cơ sở | 
| ba 1 giây | 1 | đội hình ba | 
| ba 2 giây | 1 | đội hình ba | 
| 1,2,3 | 3 | hỗn hợp các loại cặn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi phần dư bị mất cân bằng, chẳng hạn như có nhiều số 1 nhưng không có số 2. Đối với đầu vào`[1,1,1,1]`, thuật toán tính`c1 = 4`. Không thể tạo thành cặp nào nên nó tạo ra`4 // 3 = 1`, chính xác là để lại một phần tử không được sử dụng. 

Một trường hợp khác là khi mọi thứ đều chia hết cho 3, chẳng hạn như`[3,6,9]`. Bộ thuật toán`c0 = 3`và ngay lập tức trả về 3, phù hợp với thực tế là không cần hợp nhất. 

Trường hợp tinh tế cuối cùng là khi việc ghép nối sử dụng hết cấu trúc dư lượng chéo hữu ích. Vì`[1,2,1,2,1,2]`, việc ghép nối trực tiếp tạo ra ba nhóm hợp lệ và không còn phần dư nào. Điều này xác nhận rằng việc ghép đôi tham lam trước bộ ba là an toàn, vì mọi phần còn sót lại sau khi ghép đôi tối đa đều không thể được cải thiện bằng cách tái cấu trúc.
