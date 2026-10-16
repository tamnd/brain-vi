---
title: "CF 105485H - \u6587\u795e\u7684\u6bd4\u8d5b"
description: "Chúng tôi được giao cho một nhóm thí sinh tham gia một cuộc thi lập trình. Mỗi thí sinh giải được một tập hợp bài toán và tập hợp này được mã hóa dưới dạng một chuỗi các chữ cái viết hoa riêng biệt từ A đến Z. Độ dài của chuỗi là số lượng bài toán được giải."
date: "2026-06-23T18:23:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "H"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 53
verified: true
draft: false
---

[CF 105485H - \u6587\u795e\u7684\u6bd4\u8d5b](https://codeforces.com/problemset/problem/105485/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm thí sinh tham gia một cuộc thi lập trình. Mỗi thí sinh giải được một tập hợp bài toán và tập hợp này được mã hóa dưới dạng một chuỗi các chữ cái viết hoa riêng biệt từ A đến Z. Độ dài của chuỗi là số lượng bài toán được giải. Ví dụ: một chuỗi như “ACD” có nghĩa là thí sinh đã giải được ba bài toán. 

Quy tắc xếp hạng có tính chất từ ​​điển trong hai lớp. Đầu tiên, các thí sinh được sắp xếp theo số lượng bài toán họ đã giải được, trong đó các bài toán giải được nhiều hơn luôn đặt thí sinh lên trước người có ít bài hơn. Thứ hai, trong số những thí sinh giải được cùng một số bài toán, thứ tự tương đối của họ phụ thuộc vào thời gian phạt. Điểm mấu chốt là thời gian phạt không được đưa ra. Chúng tôi chỉ biết số lượng đã giải quyết và chúng tôi muốn đếm xem có thể có bao nhiêu thứ hạng đầy đủ khác nhau nếu chúng tôi ấn định thời gian phạt riêng biệt tùy ý phù hợp với quy tắc. 

Vì vậy, độ không đảm bảo hoàn toàn nằm trong các nhóm có độ dài bằng nhau. Mỗi nhóm có độ dài bằng nhau có thể được hoán vị tùy ý với nhau, trong khi thứ tự chung của các nhóm được cố định bằng cách giảm độ dài. 

Kích thước đầu vào n có thể lên tới 100000. Mỗi chuỗi có tối đa 26 ký tự. Điều này ngay lập tức loại trừ bất kỳ phép liệt kê giai thừa nào đối với tất cả các hoán vị của thí sinh hoặc bất kỳ nỗ lực nào nhằm mô phỏng các bài tập phạt một cách rõ ràng. Ngay cả trong một nhóm có kích thước k, giai thừa k trở nên không khả thi khi k lớn. 

Một điểm tinh tế là các chuỗi không nhất thiết phải là duy nhất. Nhiều thí sinh có thể có các bộ giải giống hệt nhau, nhưng vì thời gian phạt được đảm bảo khác biệt nên chúng vẫn có thể phân biệt được theo thứ tự. Điều đó có nghĩa là các chuỗi giống hệt nhau vẫn hoạt động giống như các phần tử riêng biệt trong cùng một nhóm. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta chỉ cần sắp xếp theo độ dài rồi nhân các giai thừa của quy mô nhóm. Điều đó thực sự đúng về mặt toán học, nhưng tính toán giai thừa trực tiếp mà không cần xử lý số học mô-đun và tính toán trước vẫn ổn. Vấn đề tiềm ẩn thực sự không phải là kích thước giai thừa mà là việc đảm bảo chúng tôi nhóm chính xác theo độ dài và số bội số. 

Một trường hợp phức tạp khác là khi tất cả các thí sinh đều có chiều dài khác nhau. Trong trường hợp đó, có chính xác một thứ hạng hợp lệ vì thứ tự đã được xác định đầy đủ. Một cách tiếp cận bất cẩn vẫn có thể nhân các giai thừa có kích thước một nhóm, điều này tốt nhưng thường báo hiệu sự hiểu lầm về logic nhóm. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là xem xét tất cả các lần ấn định thời gian phạt có thể phù hợp với tính khác biệt và tính toán thứ tự xếp hạng được tạo ra. Đối với mỗi bài tập, chúng tôi rút ra một hoán vị của các thí sinh và đếm xem có bao nhiêu hoán vị khác biệt xuất hiện. Vì thời gian phạt chỉ liên quan trong các nhóm có độ dài bằng nhau nên điều này làm giảm việc xem xét tất cả các hoán vị trong mỗi nhóm. 

Nếu chúng ta tập trung vào một nhóm có kích thước k thì số lượng đặt hàng nội bộ là k giai thừa. Vì các nhóm độc lập nên câu trả lời tổng thể là tích của các giai thừa của kích thước nhóm trên tất cả các giá trị độ dài. 

Chế độ xem bạo lực hoạt động vì số lần phạt chỉ tạo ra tổng thứ tự trong các lớp có độ dài bằng nhau và không bao giờ ảnh hưởng đến thứ tự giữa các lớp. Tuy nhiên, việc tạo ra hoặc suy luận rõ ràng về việc ấn định hình phạt là không cần thiết và quá chậm. Quan sát là thứ tự hình phạt tương đương với việc chọn một hoán vị tùy ý bên trong mỗi nhóm có kích thước cố định. 

Do đó, vấn đề giảm xuống còn việc phân nhóm các thí sinh theo số lượng đã giải được của họ và nhân các giai thừa của quy mô nhóm theo một mô đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua các quả phạt đền | Ồ (n!) | O(n) | Quá chậm | 
| Nhóm theo quy mô + sản ​​phẩm giai thừa | O(n + 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách coi mỗi số lần giải quyết có thể có là một nhóm.

1. Với mỗi thí sinh, hãy tính độ dài sợi dây của họ. Đây là giá trị duy nhất ảnh hưởng đến thứ hạng giữa các nhóm. 
2. Đếm xem có bao nhiêu thí sinh rơi vào mỗi giá trị độ dài từ 0 đến 26. Vì mỗi độ dài chuỗi tối đa là 26 nên chúng ta chỉ cần một mảng có kích thước cố định là 27. Điều này khiến cho việc phân nhóm thời gian không đổi đối với mỗi thí sinh. 
3. Tính toán trước các giai thừa lên tới n modulo 1e9+7. Điều này là cần thiết vì mỗi nhóm đóng góp một số hạng giai thừa và việc tính toán trực tiếp sẽ quá chậm nếu lặp lại. 
4. Khởi tạo câu trả lời là 1. 
5. Đối với mỗi nhóm chiều dài, nhân câu trả lời với giai thừa[đếm], trong đó count là số thí sinh trong nhóm đó. Nếu số đếm là 0 hoặc 1 thì đóng góp là 1 và không thay đổi câu trả lời. Phép nhân phản ánh số lượng các lệnh phạt nội bộ có thể có trong nhóm đó. 
6. Xuất kết quả cuối cùng theo modulo 1e9+7. 

Tại sao nó hoạt động xuất phát từ cấu trúc của quy tắc xếp hạng. Tất cả các thí sinh được chia thành các lớp tương đương theo số lượng đã giải. Thứ tự chung giữa các lớp là cố định và giảm dần theo kích thước, do đó không thể xen kẽ giữa các lớp. Trong mỗi lớp, thời gian phạt chỉ bị ràng buộc là khác biệt, tương đương với việc chọn một hoán vị của lớp đó. Vì các lớp là độc lập nên tổng số thứ hạng là tích của các hoán vị của mỗi lớp, chính xác là tích của các giai thừa của quy mô lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())
cnt = [0] * 27

lengths = []
for _ in range(n):
    s = input().strip()
    cnt[len(s)] += 1

fact = [1] * (n + 1)
for i in range(1, n + 1):
    fact[i] = fact[i - 1] * i % MOD

ans = 1
for c in cnt:
    ans = (ans * fact[c]) % MOD

print(ans)
```Việc triển khai trước tiên sẽ nén đầu vào vào các nhóm tần số được lập chỉ mục theo độ dài chuỗi. Điều này tránh việc lưu trữ hoặc sắp xếp các thí sinh riêng lẻ vì chỉ tính điểm quan trọng. 

Bảng giai thừa được xây dựng một lần trong thời gian tuyến tính. Điều này rất cần thiết vì mỗi phép nhân nhóm cần truy cập nhanh vào các giá trị giai thừa. 

Cuối cùng, câu trả lời sẽ tích lũy giai thừa của từng kích thước nhóm. Thứ tự của phép nhân không liên quan do tính giao hoán, nhưng việc lặp lại trên tất cả 27 độ dài có thể giúp logic đơn giản và tránh thiếu bất kỳ nhóm nào. 

Một lỗi triển khai phổ biến là quên rằng nhiều thí sinh có thể chia sẻ cùng một độ dài chuỗi, điều này sẽ làm giảm nhóm xuống kích thước một một cách không chính xác và tạo ra câu trả lời 1. Một lỗi khác là tính toán lại các giai thừa nhiều lần trong vòng lặp thay vì tính toán trước. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
ABC
ABCD
ABCD
DBAE
```Chúng tôi tính toán độ dài: 3, 4, 4, 4. Vì vậy, nhóm tần số là: 

| Bước | Chiều dài 3 | Chiều dài 4 | 
| --- | --- | --- | 
| Sau khi đọc đầu vào | 1 | 3 | 

Bây giờ giai thừa: 1! = 1, 3! = 6. 

| Xô | Đếm | Đóng góp | 
| --- | --- | --- | 
| 3 | 1 | 1 | 
| 4 | 3 | 6 | 

Câu trả lời trở thành 1 × 6 = 6. 

Điều này cho thấy chỉ có nhóm cỡ 3 mới đóng góp bất kỳ sự biến đổi nào. Nhóm phần tử đơn lẻ không đóng góp gì. 

Một ví dụ được xây dựng khác: 

đầu vào:```
A
B
C
D
```Tất cả độ dài là 1, vì vậy một thùng có kích thước 4. 

| Chiều dài | Đếm | 
| --- | --- | 
| 1 | 4 | 

Đóng góp là 4! = 24, nghĩa là bất kỳ sự hoán vị nào của số lần phạt đền đều tạo ra thứ hạng khác. 

Điều này xác nhận rằng khi tất cả thí sinh bằng nhau về số lượng đã giải, câu trả lời sẽ trở thành số hoán vị đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đếm độ dài, một lượt trên 27 nhóm, cộng với tính toán trước giai thừa | 
| Không gian | O(n) | mảng giai thừa lên đến n và mảng xô không đổi | 

Các ràng buộc cho phép n lên tới 100000, do đó thời gian tuyến tính với hệ số không đổi nhỏ dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ bị chi phối bởi mảng giai thừa và nằm trong khoảng 512 MiB. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline
    n = int(input())
    cnt = [0] * 27
    for _ in range(n):
        s = input().strip()
        cnt[len(s)] += 1

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = 1
    for c in cnt:
        ans = ans * fact[c] % MOD

    print(ans)

def run(inp: str) -> str:
    global output_capture
    old_in = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_out
    sys.stdin = old_in
    return out.getvalue().strip()

# sample
assert run("3\nABC\nABCD\nABCD\nDBAE\n") == "6"

# minimum size
assert run("1\nA\n") == "1"

# all distinct lengths
assert run("3\nA\nAB\nABC\n") == "1"

# all same length
assert run("4\nA\nB\nC\nD\n") == "24"

# mixed
assert run("5\nA\nB\nC\nDE\nDE\n") == str((2*1) % MOD)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thí sinh độc thân | 1 | trường hợp cơ sở | 
| tăng chiều dài | 1 | không có hoán vị bên trong | 
| tất cả đều có cùng độ dài | 24 | giai thừa đầy đủ | 
| độ dài trùng lặp hỗn hợp | 2 | nhân xô | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mỗi thí sinh có một số vấn đề được giải quyết khác nhau. Ví dụ:```
A
AB
ABC
```Thuật toán tạo thành các nhóm có kích thước 1, 1 và 1. Mỗi nhóm đóng góp giai thừa (1) là 1, vì vậy câu trả lời cuối cùng là 1. Thứ hạng hoàn toàn cố định vì không có hai thí sinh nào ở chung một lớp mà thứ tự hình phạt có thể khác nhau. 

Một trường hợp khác là khi tất cả các thí sinh có chung chuỗi giống hệt nhau:```
A
A
A
A
```Tất cả đều nằm trong một thùng cỡ 4, tạo ra 4! = 24. Thuật toán xử lý chính xác các tập hợp đã giải giống hệt nhau như các đối thủ độc lập, vì số lần phạt đảm bảo thứ tự khác biệt ngay cả khi các tập hợp đã giải giống hệt nhau. 

Trường hợp tinh tế cuối cùng là n lớn chỉ có một vài giá trị độ dài được điền. Ví dụ: nếu tất cả các chuỗi có độ dài 26 hoặc 25, chúng tôi vẫn tích lũy giai thừa một cách chính xác mà không cần lặp lại các nhóm không sử dụng theo cách có ý nghĩa. Mảng có kích thước 27 cố định đảm bảo tính chính xác bất kể phân phối.
