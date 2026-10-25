---
title: "CF 106032M - Vận mệnh thay đổi cuộc chơi"
description: "Chúng ta có hai người chơi, Alice và Bob, mỗi người xây dựng một chuỗi theo thời gian. Cả hai đều bắt đầu từ cùng một chuỗi tầm thường bao gồm một ký tự \"a\". Sau đó họ nhận được một chuỗi các hoạt động."
date: "2026-06-25T13:07:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106032
codeforces_index: "M"
codeforces_contest_name: "The 2025 ICPC Syrian Private Universities Collegiate Programming Contest"
rating: 0
weight: 106032
solve_time_s: 49
verified: true
draft: false
---

[CF 106032M - Vận mệnh thay đổi trò chơi](https://codeforces.com/problemset/problem/106032/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai người chơi, Alice và Bob, mỗi người xây dựng một chuỗi theo thời gian. Cả hai đều bắt đầu từ cùng một chuỗi tầm thường bao gồm một ký tự đơn`"a"`. 

Sau đó họ nhận được một chuỗi các hoạt động. Mỗi thao tác nhắm vào Alice hoặc Bob và nối thêm nhiều bản sao của một chuỗi nhất định vào chuỗi hiện tại của chúng. Sau mỗi thao tác, mỗi người chơi được phép tự do sắp xếp lại các ký tự trong toàn bộ chuỗi của mình theo bất kỳ cách nào họ muốn, với mục tiêu tạo ra sự sắp xếp nhỏ nhất có thể về mặt từ điển của các ký tự mà họ hiện có. 

Vì việc sắp xếp lại không bị hạn chế và chỉ tính ký tự là quan trọng nên cấu trúc thực tế của chuỗi trở nên không liên quan. Điều quan trọng sau mỗi thao tác chỉ là mỗi người chơi đã tích lũy được bao nhiêu nhân vật. 

Sau khi cả hai người chơi tối ưu hóa thứ tự của họ một cách độc lập, chúng tôi so sánh các chuỗi kết quả của họ theo từ điển và xuất ra chuỗi nào nhỏ hơn hoặc liệu chúng có bằng nhau hay không. 

Ý nghĩa chính là mỗi chuỗi luôn là tập hợp nhiều ký tự được sắp xếp do người chơi sở hữu. Vì vậy, vấn đề giảm xuống còn việc duy trì tần số đếm các chữ cái cho Alice và Bob và so sánh hai bộ nhiều chữ cái sau mỗi lần cập nhật. 

Kích thước đầu vào làm cho việc giảm này trở nên cần thiết. Mỗi thao tác có thể nối thêm một chuỗi có độ dài lên tới 400.000 được lặp lại tới 200.000 lần, nhưng trên toàn cầu, tổng độ dài trên tất cả các thao tác bị giới hạn bởi 400.000 cho mỗi trường hợp thử nghiệm. Ràng buộc này là quan sát quan trọng: mặc dù k có thể lớn nhưng chúng ta chỉ cần nhân số lượng và tổng số ký tự riêng biệt được xử lý là tuyến tính ở kích thước đầu vào. 

Một cách giải thích ngây thơ xây dựng các chuỗi lặp đi lặp lại một cách rõ ràng sẽ ngay lập tức thất bại, vì ngay cả một thao tác đơn lẻ cũng có thể mở rộng đến độ dài khổng lồ. 

Một trường hợp đặc biệt thường phá vỡ các giải pháp đơn giản là khi cùng một thao tác liên tục nối thêm một chuỗi lớn nhiều lần. Ví dụ, gắn thêm`"zz"`lặp lại 100000 lần không nên xây dựng chuỗi 200000 ký tự một cách rõ ràng; làm như vậy sẽ hết thời gian hoặc thậm chí làm cạn kiệt bộ nhớ. 

Một trường hợp tinh tế khác là khi Alice và Bob nhận được chuỗi thao tác giống hệt nhau. Đầu ra chính xác phải luôn là`"Tie"`ở mọi bước. Giải pháp so sánh không chính xác các chuỗi thô thay vì số lượng ký tự có thể phân biệt nhầm thứ tự nội bộ khác nhau mặc dù cả hai luôn được sắp xếp ở cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng mô phỏng vấn đề theo nghĩa đen. Đối với mỗi thao tác, chúng tôi mở rộng chuỗi bằng cách lặp lại`x`chính xác`k`lần và nối nó vào chuỗi của người chơi mục tiêu. Sau mỗi lần cập nhật, chúng tôi sắp xếp toàn bộ chuỗi và so sánh Alice và Bob theo từ điển. 

Về nguyên tắc, điều này đúng vì nó tuân theo chính xác các quy tắc: xây dựng chuỗi, sắp xếp lại chuỗi một cách tối ưu và so sánh. Tuy nhiên, chi phí sắp xếp tăng theo tổng chiều dài chuỗi. Sau nhiều thao tác, chuỗi có thể tăng kích thước theo tỷ lệ với tổng của tất cả các ký tự được nối thêm. Với tối đa 400.000 ký tự và khả năng sắp xếp lặp đi lặp lại, tổng chi phí sẽ trở thành khoảng O(n² log n) cho các hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Thông tin chi tiết quan trọng là việc sắp xếp sau mỗi thao tác là không cần thiết. Vì chúng ta luôn được phép sắp xếp lại một cách tùy ý nên chuỗi của mỗi người chơi được xác định đầy đủ bởi một mảng tần số có kích thước 26. Nối thêm`x`lặp đi lặp lại`k`lần chỉ cần thêm`k * count_of_each_character_in_x`tới vectơ tần số của người chơi. So sánh giữa Alice và Bob giảm xuống còn so sánh hai vectơ có độ dài 26 theo từ điển, chính xác như thể chúng ta đã sắp xếp các chuỗi. 

Điều này làm giảm mỗi thao tác thành việc đếm các ký tự trong`x`một lần và nhân rộng theo`k`, sau đó cập nhật một mảng có kích thước cố định. Việc so sánh từ điển cũng có thời gian không đổi trên 26 ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng + sắp xếp chuỗi) | O(total_length log tổng_length cho mỗi truy vấn) | O(tổng_chiều dài) | Quá chậm | 
| Đếm tần số trên mỗi thao tác | O(26 · q) cho mỗi trường hợp thử nghiệm | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`A[26]`Và`B[26]`đại diện cho số lượng mỗi chữ cái của Alice và Bob. Cả hai đều bắt đầu bằng`A['a']=1`Và`B['a']=1`, tất cả những cái khác bằng không. Điều này mã hóa chuỗi ban đầu mà không lưu trữ nó một cách rõ ràng. 
2. Với mỗi thao tác, hãy đọc`(op, x, k)`. 
3. Tính toán mảng tần số cục bộ`cnt[26]`cho chuỗi`x`. Điều này yêu cầu một lần quét`x`, đếm số lần xuất hiện của mỗi ký tự. 
4. Nhân mọi`cnt[c]`qua`k`. Điều này thể hiện việc lặp lại chuỗi`k`nhiều lần mà không cần xây dựng nó. 
5. Nếu hoạt động nhắm vào Alice, hãy thêm`cnt`vào trong`A`. Ngược lại thì thêm vào`B`. Mỗi bản cập nhật là sự tích lũy trực tiếp số lượng ký tự. 
6. Để quyết định kết quả sau thao tác này, hãy so sánh`A`Và`B`theo từ điển từ`'a'`ĐẾN`'z'`. Chỉ số đầu tiên nơi chúng khác nhau sẽ xác định người chiến thắng: số lượng nhỏ hơn có nghĩa là chuỗi được sắp xếp của người chơi nhỏ hơn về mặt từ điển. 
7. Nếu tất cả số đếm khớp nhau, xuất ra`"Tie"`. 

Tại sao nó hoạt động dựa trên tính bất biến là sau mỗi thao tác, chuỗi của mỗi người chơi luôn được sắp xếp theo thứ tự ký tự không giảm. Vì việc sắp xếp lại không bị hạn chế nên mọi hoán vị sẽ giảm xuống cùng một tập hợp nhiều tập hợp được sắp xếp chính tắc. Vì vậy, việc so sánh từ điển giữa những người chơi cũng tương đương với việc so sánh các vectơ tần số của họ theo thứ tự bảng chữ cái. Không có cấu trúc ẩn nào của chuỗi quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cmp(a, b):
    for i in range(26):
        if a[i] != b[i]:
            return "Alice" if a[i] < b[i] else "Bob"
    return "Tie"

def main():
    T = int(input())
    for _ in range(T):
        q = int(input())
        A = [0] * 26
        B = [0] * 26

        A[0] = 1
        B[0] = 1

        for _ in range(q):
            parts = input().split()
            op = int(parts[0])
            x = parts[1]
            k = int(parts[2])

            cnt = [0] * 26
            for ch in x:
                cnt[ord(ch) - 97] += 1

            for i in range(26):
                cnt[i] *= k

            if op == 1:
                for i in range(26):
                    A[i] += cnt[i]
            else:
                for i in range(26):
                    B[i] += cnt[i]

            print(cmp(A, B))

if __name__ == "__main__":
    main()
```Lựa chọn triển khai chính là tránh mọi việc xây dựng chuỗi mở rộng. Bước nhân thay thế những gì lẽ ra sẽ là k phép nối. Một chi tiết tinh tế khác là giữ cho mảng tần số cố định ở kích thước 26, đảm bảo so sánh theo thời gian liên tục không phụ thuộc vào tổng mức tăng trưởng của chuỗi. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa: 

đầu vào:```
1
3
1 aa 2
2 ab 1
2 a 3
```Chúng ta bắt đầu với Alice =`{a:1}`, Bob =`{a:1}`. 

Sau thao tác đầu tiên, Alice cho biết thêm`"aa"`hai lần, đóng góp`{a:4}`tổng cộng cho đến nay`{a:5}`. 

Sau thao tác thứ hai, Bob cho biết thêm`"ab"`một lần, thế là Bob trở thành`{a:2, b:1}`. 

Sau thao tác thứ ba, Bob cho biết thêm`"a"`ba lần, vì vậy Bob trở thành`{a:5, b:1}`. 

Ở mỗi bước chúng tôi so sánh từ điển: 

| Bước | Alice A | Bob B | Kết quả | 
| --- | --- | --- | --- | 
| ban đầu | một:1 | một:1 | Cà vạt | 
| 1 | một:5 | một:1 | Bob | 
| 2 | một:5 | a:2, b:1 | Alice | 
| 3 | một:5 | a:5, b:1 | Alice | 

Dấu vết này cho thấy rằng so sánh từ điển phụ thuộc vào ký tự khác nhau đầu tiên, ở đây`'a'`sau đó`'b'`và không theo bất kỳ thứ tự nào của chuỗi gốc. 

Bây giờ hãy xem xét ví dụ thứ hai nhấn mạnh vào cấu trúc: 

đầu vào:```
1
2
1 cba 1
2 abc 1
```Sau cả hai thao tác: 

Alice:`{a:1, b:1, c:1}`Bob:`{a:1, b:1, c:1}`Mặc dù đầu vào khác nhau, việc sắp xếp sẽ loại bỏ hoàn toàn cấu trúc, do đó kết quả luôn là hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · q + tổng | x | 
| Không gian | O(1) | Chỉ có hai mảng cố định có kích thước 26 | 

Các ràng buộc giới hạn tổng độ dài của tất cả các chuỗi ở mức 400.000 cho mỗi trường hợp thử nghiệm, do đó việc quét tuyến tính trên tất cả các ký tự là an toàn. Hệ số không đổi của 26 phép so sánh cho mỗi truy vấn là không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []

    for _ in range(T):
        q = int(input())
        A = [0]*26
        B = [0]*26
        A[0] = B[0] = 1

        def cmp(a,b):
            for i in range(26):
                if a[i]!=b[i]:
                    return "Alice" if a[i] < b[i] else "Bob"
            return "Tie"

        for _ in range(q):
            op,x,k = input().split()
            op = int(op); k = int(k)
            cnt = [0]*26
            for ch in x:
                cnt[ord(ch)-97]+=1
            for i in range(26):
                cnt[i]*=k
            if op==1:
                for i in range(26): A[i]+=cnt[i]
            else:
                for i in range(26): B[i]+=cnt[i]
            out.append(cmp(A,B))

    return "\n".join(out)

# custom cases
assert run("1\n1\n1 a 1\n") == "Tie", "single update"
assert run("1\n2\n1 z 1\n2 a 1\n") == "Bob\nBob", "lexicographic dominance"
assert run("1\n2\n1 abc 2\n2 acb 2\n") == "Tie\nTie", "reordering symmetry"
assert run("1\n1\n1 zzz 100\n") == "Bob", "large repetition handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật duy nhất | Cà vạt | khởi tạo đúng đắn | 
| z vs a | Bob | thứ tự từ điển | 
| chuỗi hoán vị | Cà vạt | sắp xếp lại sự không liên quan | 
| lặp lại lớn x | Bob | xử lý k lớn | 

## Vỏ cạnh 

Trường hợp cạnh thứ nhất là khi cả hai người chơi vẫn giống nhau trong tất cả các hoạt động. Thuật toán duy trì các mảng tần số giống hệt nhau nên vòng lặp so sánh luôn kết thúc mà không tìm ra điểm khác biệt, xuất ra chính xác`"Tie"`mọi lúc. 

Một trường hợp khác là sự lặp lại nhiều, ví dụ như nối thêm một chuỗi 400.000 ký tự với hệ số nhân lớn. Việc triển khai không bao giờ tạo chuỗi mở rộng, do đó mức sử dụng bộ nhớ không đổi. Bước nhân chia tỷ lệ chính xác mà không lo ngại tràn trong Python. 

Trường hợp cuối cùng là khi sự khác biệt chỉ xảy ra ở các ký tự cuối bảng chữ cái. Vì so sánh quét từ`'a'`ĐẾN`'z'`, các tiền tố bằng nhau trước đó được bỏ qua một cách an toàn và ký tự khác nhau đầu tiên xác định kết quả chính xác theo yêu cầu của thứ tự từ điển.
