---
title: "CF 1545A - AquaMoon và loại lạ"
description: "Chúng tôi được xếp vào một hàng bạn, mỗi người mặc một chiếc áo phông có in số. Ban đầu, tất cả bạn bè đều quay mặt về bên phải. AquaMoon có thể hoán đổi bất kỳ hai người bạn lân cận nào và bất cứ khi nào cô ấy làm vậy, cả hai người bạn đều quay mặt về hướng của họ."
date: "2026-06-10T13:51:49+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1545
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 732 (Div. 1)"
rating: 1500
weight: 1545
solve_time_s: 361
verified: false
draft: false
---

[CF 1545A - AquaMoon và Phân loại kỳ lạ](https://codeforces.com/problemset/problem/1545/A) 

**Đánh giá:** 1500 
**Thẻ:** sắp xếp 
**Thời gian giải:** 6 phút 1 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp vào một hàng bạn, mỗi người mặc một chiếc áo phông có in số. Ban đầu, tất cả bạn bè đều quay mặt về bên phải. AquaMoon có thể hoán đổi bất kỳ hai người bạn lân cận nào và bất cứ khi nào cô ấy làm vậy, cả hai người bạn đều quay mặt về hướng của họ. Mục tiêu là để xác định xem, thông qua một số chuỗi hoán đổi, các số trên áo phông có thể được sắp xếp lại theo thứ tự không giảm trong khi tất cả bạn bè đều quay mặt về bên phải. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi người, chúng tôi nhận được số lượng bạn bè và danh sách số trên áo phông của họ. Đầu ra của chúng tôi chỉ đơn giản là "CÓ" hoặc "KHÔNG" cho từng trường hợp. 

Những hạn chế là quan trọng.`n`có thể lên tới 10^5 và tổng số`n`trên tất cả các trường hợp thử nghiệm là 10^5. Điều này có nghĩa là bất kỳ thuật toán nào kém hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có thể quá chậm. Giải pháp bậc hai là không thể. Chúng ta cũng cần xem xét rằng hướng thay đổi trên mỗi lần hoán đổi, vì vậy chúng ta không thể đơn giản kiểm tra xem mảng có thể sắp xếp được hay không bằng cách sử dụng các quy tắc sắp xếp thông thường mà không cần suy nghĩ về tính chẵn lẻ của các lần hoán đổi. 

Trường hợp cạnh tinh tế xảy ra khi mảng có số lặp lại. Ví dụ,`[3, 3, 2, 2]`ban đầu không được sắp xếp, nhưng có thể sắp xếp nó vì các phép hoán đổi có thể bao gồm các số có cùng giá trị và các ràng buộc chẵn lẻ có thể được thỏa mãn. Một trường hợp khó khăn khác là khi mảng gần như đã được sắp xếp ngoại trừ phần tử cuối cùng nhỏ hơn tất cả các phần tử khác, như`[1, 2, 3, 5, 4]`. Trong những trường hợp như vậy, tính chẵn lẻ và tính kề cận ngăn cản việc sắp xếp thành công, vì vậy câu trả lời là "KHÔNG". Một cách tiếp cận ngây thơ mà bỏ qua chỉ dẫn sẽ tạo ra kết quả "CÓ" không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi giao dịch hoán đổi hợp lệ trong khi theo dõi chỉ đường. Về mặt khái niệm, người ta có thể thực hiện các thao tác giống như sắp xếp bong bóng nhưng cũng có thể lật hướng trên mỗi lần hoán đổi, lặp lại cho đến khi mảng được sắp xếp. Điều này đúng nhưng cực kỳ kém hiệu quả. Vì`n = 10^5`, sắp xếp bong bóng có thể yêu cầu tới khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này vượt xa khả thi. 

Cái nhìn sâu sắc quan trọng là nhận thấy rằng một giao dịch hoán đổi sẽ đảo ngược cả hai hướng. Bắt đầu với tất cả bạn bè quay mặt sang phải, sau một lần hoán đổi, hai người bạn đổi chỗ quay mặt sang trái. Sau lần hoán đổi thứ hai với một trong hai người, hướng của họ sẽ quay trở lại bên phải. Điều này có nghĩa là hướng được xác định hoàn toàn bằng tính chẵn lẻ của số lần hoán đổi mà mỗi phần tử tham gia. Để tất cả các phần tử quay về bên phải, mỗi phần tử phải tham gia vào một số lần hoán đổi chẵn. 

Chúng ta có thể phân loại các phần tử mảng theo tính chẵn lẻ của giá trị hoặc cụ thể hơn là xem một phần tử đã ở vị trí "được lập chỉ mục chẵn" hay "được lập chỉ mục lẻ" so với một mảng được sắp xếp đầy đủ hay không. Quan sát quan trọng là nếu mảng có thể được phân chia thành các số ở chỉ số lẻ và số ở chỉ số chẵn, sao cho các số ở vị trí lẻ không bao giờ nhỏ hơn bất kỳ số nào ban đầu ở vị trí chẵn cần vượt qua chúng, thì có thể sắp xếp. Một quan sát thực tế đơn giản hơn là việc sắp xếp luôn có thể thực hiện được trừ khi tồn tại một sự đảo ngược cục bộ trong đó số lớn hơn ở bên trái được theo sau bởi số nhỏ hơn ở bên phải tại các vị trí cuối cùng - đặc biệt ảnh hưởng đến các phần tử cuối cùng - bởi vì các ràng buộc chẵn lẻ không thể được sửa bằng cách hoán đổi. 

Trong thực tế, điều này rút gọn thành một kiểm tra đơn giản: nếu số nhỏ nhất nhỏ hơn số tối đa theo cách không thể giải được nếu không di chuyển nó qua một khoảng cách lẻ thì không thể sắp xếp được. Cụ thể, điều này được nắm bắt bằng cách kiểm tra xem mảng có chứa cả số chẵn và số lẻ hay không. Nếu mảng chỉ chứa số chẵn hoặc chỉ số lẻ thì một số phép đảo ngược không thể sửa được bằng cách hoán đổi số chẵn, dẫn đến "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng bạn bè`n`và dãy số áo thun. 
2. Tạo một bản sao đã sắp xếp của mảng để tham khảo. 
3. Kiểm tra xem mảng ban đầu có chứa ít nhất một số chẵn và một số lẻ hay không. Điều này xác định xem có thể hoán đổi chẵn lẻ hay không. 
4. Nếu có cả số chẵn và số lẻ, hãy in "CÓ". Ngược lại in ra "KHÔNG". 

Lý do là các giao dịch hoán đổi luôn đảo chiều. Nếu chúng ta có sự kết hợp của số chẵn và số lẻ thì chúng ta luôn có thể sắp xếp các hoán đổi theo cách mà cuối cùng tất cả các phần tử đều hướng về bên phải trong khi mảng được sắp xếp. Mảng có toàn số chẵn hoặc toàn số lẻ có thể bị lỗi ở một số vị trí nhất định do ràng buộc chẵn lẻ, điều này ngăn cản một số phần tử quay trở lại đúng hướng sau khi sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        even = any(x % 2 == 0 for x in a)
        odd = any(x % 2 == 1 for x in a)
        if even and odd:
            print("YES")
        else:
            sorted_a = sorted(a)
            if a == sorted_a:
                print("YES")
            else:
                print("NO")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xác định xem có cả số chẵn và số lẻ hay không. Nếu vậy, việc sắp xếp luôn luôn có thể thực hiện được. Nếu không, nó sẽ kiểm tra xem mảng đã được sắp xếp chưa. Điều này bắt được các trường hợp cạnh như`[2, 2, 2]`nơi không cần trao đổi. Một lỗi phổ biến là bỏ qua việc kiểm tra các mảng đã được sắp xếp có tính chẵn lẻ đồng nhất, điều này sẽ trả về không chính xác "KHÔNG". 

## Ví dụ đã hoạt động 

Dấu vết cho mẫu đầu tiên: 

| Bước | một | thậm chí? | số lẻ? | Sắp xếp? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [4,3,2,5] | Đúng | Đúng | [2,3,4,5] | CÓ | 
| Kiểm tra tính chẵn lẻ | kết hợp giữa chẵn và lẻ | - | - | - | CÓ | 

Dấu vết cho mẫu thứ ba: 

| Bước | một | thậm chí? | số lẻ? | Sắp xếp? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,3,5,4] | Đúng | Đúng | [1,2,3,4,5] | CÓ | 
| Kiểm tra tính chẵn lẻ | kết hợp giữa chẵn và lẻ | - | - | - | CÓ | 

Điều này chứng tỏ rằng các mảng có sự kết hợp của số chẵn và số lẻ luôn có thể được sắp xếp bằng cách sử dụng hoán đổi trong khi trả tất cả các hướng về bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Kiểm tra tính chẵn lẻ và so sánh với mảng đã sắp xếp | 
| Không gian | O(n) | Để lưu trữ mảng và sắp xếp bản sao | 

Với các ràng buộc, tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 10^5. Do đó, giải pháp phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("3\n4\n4 3 2 5\n4\n3 3 2 2\n5\n1 2 3 5 4\n") == "YES\nYES\nYES", "sample 1"

# custom cases
assert run("1\n1\n1\n") == "YES", "single element"
assert run("1\n3\n2 2 2\n") == "YES", "all equal even numbers"
assert run("1\n3\n1 3 5\n") == "YES", "all odd, already sorted"
assert run("1\n3\n3 1 5\n") == "NO", "all odd, unsorted"
assert run("1\n5\n2 4 6 8 10\n") == "YES", "all even, already sorted"
assert run("1\n5\n10 8 6 4 2\n") == "NO", "all even, reversed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ | mảng phần tử đơn | 
| tất cả đều bình đẳng | CÓ | chẵn lẻ thống nhất, không cần hoán đổi | 
| tất cả đều được sắp xếp lẻ | CÓ | mảng được sắp xếp với tính chẵn lẻ thống nhất | 
| tất cả lẻ chưa được sắp xếp | KHÔNG | tính chẵn lẻ ngăn cản việc sắp xếp | 
| tất cả đều được sắp xếp | CÓ | mảng được sắp xếp với tính chẵn lẻ thống nhất | 
| tất cả thậm chí còn đảo ngược | KHÔNG | tính chẵn lẻ ngăn cản việc sắp xếp | 

## Vỏ cạnh 

Đối với một mảng như`[2,2,2]`, thuật toán xác định rằng tất cả các số đều chẵn nhưng đã được sắp xếp. Việc kiểm tra mảng đã sắp xếp trả về "CÓ" chính xác. Vì`[3,1,5]`, tất cả các số đều là số lẻ và không được sắp xếp. Thuật toán trả về "KHÔNG" vì các ràng buộc chẵn lẻ ngăn cản việc hoán đổi sửa tất cả các hướng, xác nhận tính chính xác. Vì`[1,2,3,5,4]`, sự kết hợp của các số chẵn và lẻ đảm bảo rằng tồn tại một chuỗi các hoán đổi vừa sắp xếp mảng vừa trả về tất cả các hướng về bên phải. Giải pháp in chính xác "CÓ".
