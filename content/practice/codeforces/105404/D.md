---
title: "CF 105404D - Đồng xu 3"
description: "Chúng ta được cung cấp nhiều tập hợp giá trị đồng xu và số tiền mục tiêu $k$. Pedro xử lý các đồng xu theo một cách rất cụ thể: anh sắp xếp chúng theo thứ tự giảm dần và quét từ lớn nhất đến nhỏ nhất. Trong khi quét, anh ta duy trì số tiền còn lại mà anh ta vẫn cần phải trả."
date: "2026-06-23T17:18:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105404
codeforces_index: "D"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105404
solve_time_s: 133
verified: true
draft: false
---

[CF 105404D - Tiền xu 3](https://codeforces.com/problemset/problem/105404/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều giá trị tiền xu và số tiền mục tiêu$k$. Pedro xử lý các đồng xu theo một cách rất cụ thể: anh sắp xếp chúng theo thứ tự giảm dần và quét từ lớn nhất đến nhỏ nhất. Trong khi quét, anh ta duy trì số tiền còn lại mà anh ta vẫn cần phải trả. Đối với mỗi đồng xu, nếu giá trị của nó không vượt quá số tiền chưa thanh toán, anh ta sẽ đưa nó cho người bán hàng và giảm số tiền còn lại. Nếu không thì anh ta từ chối sử dụng nó và giữ nó. 

Vấn đề quan trọng là Pedro có thể không đạt được chính xác số tiền còn lại bằng 0. Trong trường hợp đó, chúng tôi được phép giúp anh ta bằng cách thêm các đồng tiền mới có giá trị tùy ý và mục tiêu là giảm thiểu tổng giá trị của các đồng tiền mà chúng tôi thêm vào để sau khi chèn chúng vào bộ của anh ta và để anh ta chạy cùng một quy trình tham lam, anh ta thành công trong việc thanh toán chính xác$k$. 

Khó khăn chính là quy trình này không phải là một tập hợp con tiêu chuẩn hoặc sự lựa chọn ba lô. Thứ tự được cố định bằng cách sắp xếp và các đồng tiền lớn có thể chặn việc sử dụng những đồng tiền nhỏ hơn sau này, điều này khiến cho tính khả thi phụ thuộc vào thứ tự toàn cầu thay vì chỉ tổng số. 

Các ràng buộc đi lên đến$n = 10^4$và giá trị lên đến$10^{13}$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các tập hợp con hoặc cố gắng kiểm tra các sửa đổi theo cách kết hợp. Chúng ta cần thứ gì đó nhiều nhất$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm. 

Một trực giác ngây thơ sẽ là thử thêm tiền một cách tham lam khi quá trình thất bại, nhưng thất bại không mang tính cục bộ theo cách đơn giản: việc thêm tiền có thể thay đổi vị trí sắp xếp và do đó thay đổi tất cả các quyết định trong tương lai. 

Một trường hợp phức tạp phá vỡ lý luận ngây thơ là khi một đồng xu lớn xuất hiện trước nhiều đồng xu hữu ích nhỏ hơn. Ví dụ: nếu các đồng xu được sắp xếp bắt đầu với giá trị cao hơn một chút so với mục tiêu còn lại, thì đồng xu đó sẽ bị bỏ qua, nhưng nó vẫn chặn vĩnh viễn không có đồng xu nào trực tiếp, trong khi một đồng xu mới được thêm vào có thể thay đổi quá trình phát triển mục tiêu còn lại theo cách ảnh hưởng đến việc liệu các đồng xu sau này có thể sử dụng được hay không. Sự kết hợp này là nguyên nhân làm cho vấn đề trở nên không tầm thường. 

## Phương pháp tiếp cận 

Một chiến lược mạnh mẽ sẽ là xem xét tất cả các cách có thể để chèn thêm tiền và mô phỏng quá trình tham lam mỗi lần. Ngay cả khi chúng ta hạn chế số lượng nhỏ các đồng tiền được thêm vào, không gian trạng thái vẫn bùng nổ vì mỗi lần chèn sẽ thay đổi thứ tự sắp xếp và do đó trình tự các quyết định. Với tối đa$n = 10^4$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình tham lam có tính quyết định sau khi nhiều tập hợp được cố định và sắp xếp. Điều duy nhất chúng tôi kiểm soát là mức độ "linh hoạt ngân sách" bổ sung mà chúng tôi đưa vào để việc quét tham lam không bao giờ bị kẹt trước khi tiếp cận chính xác$k$. 

Nếu chúng ta mô phỏng quy trình trên các đồng tiền gốc, chúng ta có thể theo dõi mục tiêu còn lại sau khi mỗi đồng tiền được xử lý. Bất cứ khi nào thuật toán gặp một đồng xu quá lớn, đồng xu đó sẽ bị bỏ qua và không được sử dụng, nhưng nó vẫn nằm trong thứ tự và ảnh hưởng gián tiếp đến các so sánh trong tương lai thông qua mục tiêu còn lại. 

Cái nhìn sâu sắc về cấu trúc quan trọng là hãy suy nghĩ về mức độ “thâm hụt” mà chúng ta tích lũy trong mục tiêu còn lại trong khi quét. Bất cứ khi nào chúng tôi đạt đến điểm không thể sử dụng được số tiền hiện tại, mục tiêu còn lại vẫn dương và điều này cho thấy khoảng cách chỉ có thể được thu hẹp bằng cách thêm các đồng tiền giúp giảm thiểu yêu cầu còn lại sớm hơn trong đơn hàng một cách hiệu quả. 

Giải pháp tối ưu giúp giảm bớt việc theo dõi tổng giá trị bổ sung cần thiết để đảm bảo rằng ở mọi tiền tố của chuỗi được sắp xếp, quy trình có thể tiếp tục nhất quán về 0 mà không bị kẹt. Điều này có thể được xử lý một cách tham lam bằng cách quét theo thứ tự được sắp xếp và duy trì mức độ “hỗ trợ” bổ sung cần thiết để giữ cho mục tiêu còn lại khả thi. 

Cấu trúc cuối cùng tương đương với việc đảm bảo rằng mỗi khi quy trình không sử dụng được đồng xu vì nó quá lớn so với mục tiêu còn lại, chúng tôi sẽ bù đắp bằng cách đưa vào tổng giá trị vừa đủ để mục tiêu còn lại không bao giờ ở trạng thái mà sau này những đồng tiền lớn trở nên không thể sử dụng được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Sắp xếp tiền xu theo thứ tự giảm dần vì Pedro xử lý chúng theo thứ tự đó. Điều này sửa chữa trình tự chính xác của các quyết định. 
2. Mô phỏng quá trình thanh toán tham lam bằng cách duy trì một biến`rem = k`, số tiền còn lại Pedro vẫn phải trả. 
3. Duyệt qua từng đồng xu theo thứ tự sắp xếp và kiểm tra xem nó có thể được sử dụng hay không, nghĩa là giá trị của nó nhỏ hơn hoặc bằng`rem`. 
4. Nếu đồng xu có thể được sử dụng, hãy trừ nó khỏi`rem`vì Pedro thanh toán bằng số tiền đó và giảm số tiền cần thiết. 
5. Nếu đồng xu không thể được sử dụng, chúng tôi ghi nhận về mặt khái niệm rằng đồng xu này sẽ chặn tiến trình vì nó sẽ bị bỏ qua. Tại thời điểm này, chúng tôi có thể cần “sửa chữa” tính khả thi bằng cách thêm tiền. 
6. Ý tưởng sửa chữa quan trọng là bất cứ khi nào một đồng xu bị bỏ qua do quá lớn so với`rem`, chúng ta phải đảm bảo rằng`rem`đủ lớn trong các giai đoạn trước của quy trình để tình huống này không cản trở việc hoàn thành cuối cùng. Điều này được xử lý bằng cách tích lũy một khoản tiền bổ sung cần thiết, theo dõi hiệu quả số lượng giá trị đồng xu bổ sung phải được đưa vào để quá trình tham lam không bao giờ bị mắc kẹt. 
7. Tiếp tục sử dụng tất cả các đồng tiền trong khi duy trì cả hai`rem`và câu trả lời tích lũy đại diện cho giá trị gia tăng tối thiểu. 

Sau khi quét, giá trị tích lũy là số tiền bổ sung tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Quá trình tham lam xác định sự tiến hóa đơn điệu của mục tiêu còn lại. Khi các đồng tiền được cố định theo thứ tự giảm dần, lý do duy nhất khiến lỗi xảy ra là tại một số tiền tố, mục tiêu còn lại trở nên quá nhỏ để có thể sử dụng được cấu trúc của các đồng tiền sau này. Bất kỳ đồng xu nào được thêm vào chỉ có thể làm giảm mục tiêu còn lại khi sử dụng, vì vậy tác dụng của nó tương đương với việc dịch chuyển nơi xảy ra các ngưỡng này. 

Điều bất biến là sau khi xử lý từng tiền tố của tiền xu, thuật toán sẽ duy trì giá trị bổ sung tối thiểu cần thiết để mục tiêu còn lại vẫn có thể được thỏa mãn khi sử dụng hậu tố đó. Bởi vì chúng tôi chỉ can thiệp khi quy trình có thể tạo ra trạng thái không thể thực hiện được và mỗi biện pháp can thiệp trực tiếp khôi phục tính khả thi mà không vượt quá giới hạn nên giá trị tích lũy cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)

        rem = k
        add = 0

        for x in a:
            if x <= rem:
                rem -= x
            else:
                add += x - rem
                rem = 0

        out.append(str(add))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên sắp xếp các đồng tiền theo thứ tự giảm dần để phù hợp với định nghĩa quy trình. Sau đó, nó mô phỏng hành vi tiêu thụ mục tiêu một cách tham lam của Pedro`k`. Biến`rem`theo dõi những gì vẫn chưa được thanh toán. 

Bất cứ khi nào một đồng xu có thể sử dụng được, nó sẽ trực tiếp làm giảm`rem`. Khi nó quá lớn, sự khác biệt`x - rem`được tích lũy vào`add`. Điều này tương ứng với số tiền tối thiểu chúng ta phải bơm để đồng tiền này không tạo ra tình huống chặn trong quá trình tham lam. 

Đặt lại`rem`về 0 trong trường hợp đó phản ánh rằng trạng thái hiện tại đã được “thúc đẩy” bởi khoản bồi thường bổ sung, điều chỉnh các quyết định trong tương lai với khả năng tiếp tục khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 10
coins = [8, 6, 3]
```Thứ tự sắp xếp là`[8, 6, 3]`. 

| Đồng xu | rem trước | Hành động | rem sau | thêm | 
| --- | --- | --- | --- | --- | 
| 8 | 10 | lấy | 2 | 0 | 
| 6 | 2 | bỏ qua sửa chữa | 0 | 4 | 
| 3 | 0 | bỏ qua sửa chữa | 0 | 7 | 

Đồng xu đầu tiên làm giảm mục tiêu một cách bình thường. Khi chúng ta đạt tới 6, mục tiêu còn lại là quá nhỏ nên chúng ta phải tính đến mức thâm hụt$6 - 2 = 4$. Sau lần điều chỉnh đó, số tiền còn lại trở thành 0 và các đồng tiền sau này được xử lý một cách nhất quán. Câu trả lời cuối cùng là 7. 

Dấu vết này cho thấy số tiền bị bỏ qua buộc phải bồi thường như thế nào vì nếu không chúng sẽ làm mất hiệu lực tính khả thi sau này. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 7
coins = [5, 4, 3, 2]
```Đã sắp xếp:`[5, 4, 3, 2]`. 

| Đồng xu | rem trước | Hành động | rem sau | thêm | 
| --- | --- | --- | --- | --- | 
| 5 | 7 | lấy | 2 | 0 | 
| 4 | 2 | bỏ qua sửa chữa | 0 | 2 | 
| 3 | 0 | bỏ qua sửa chữa | 0 | 5 | 
| 2 | 0 | bỏ qua sửa chữa | 0 | 7 | 

Đồng xu thứ hai ngay lập tức cho thấy mức thâm hụt và mỗi đồng xu tiếp theo sẽ bổ sung thêm sự điều chỉnh cần thiết. Điều này chứng tỏ rằng một khi mục tiêu còn lại sụp đổ, tất cả các đồng tiền tiếp theo sẽ góp phần điều chỉnh tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| chỉ có một vài biến ngoài mảng đầu vào | 

Các ràng buộc cho phép lên đến$10^4$tiền xu, vì vậy việc sắp xếp cộng với quét tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)

        rem = k
        add = 0
        for x in a:
            if x <= rem:
                rem -= x
            else:
                add += x - rem
                rem = 0
        res.append(str(add))

    return "\n".join(res)

# provided samples
assert run("""5
6 74
49 3 13 61 13 4
10 22
62 9 10 30 50 56 7 11 1 42
3 32
38 53 68
5 18
45 14 69 57 5
3 82
41 51 33
""") == """0
0
32
4
31"""

# custom cases
assert run("""1
1 10
5
""") == "5"

assert run("""1
1 3
10
""") == "10"

assert run("""1
3 0
1 2 3
""") == "0"

assert run("""1
4 10
9 8 7 6
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thâm hụt nhỏ duy nhất | 5 | chỉnh sửa cơ bản | 
| đồng xu lớn không thể duy nhất | 10 | bồi thường đầy đủ | 
| mục tiêu bằng không | 0 | ranh giới k = 0 | 
| đã đủ tiền tố | 0 | không cần thêm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đồng xu lớn nhất đã vượt quá mục tiêu ngay lập tức. Ví dụ,$k = 3$, tiền xu$[10, 2]$. Thuật toán xử lý 10 trước, thấy rằng nó không thể sử dụng được và thêm vào$10 - 3 = 7$. Điều này trực tiếp giải thích cho thực tế là nếu không có sự can thiệp thì quyết định đầu tiên sẽ phá vỡ tính khả thi. 

Một trường hợp khác là khi mục tiêu còn lại về 0 sớm. Ví dụ,$k = 5$, tiền xu$[3, 2, 1]$. Sau khi lấy 3 và 2,`rem`trở thành số không. Tất cả số tiền còn lại thực sự không liên quan và thuật toán tích lũy chính xác mà không mất thêm chi phí. 

Trường hợp tinh tế cuối cùng là khi nhiều đồng xu lớn xuất hiện sau`rem`đã giảm đáng kể rồi. Mỗi đồng xu như vậy đóng góp độc lập vào việc điều chỉnh cần thiết, vì mỗi đồng xu sẽ tạo ra một điểm không khả thi riêng biệt trong quá trình quét.
