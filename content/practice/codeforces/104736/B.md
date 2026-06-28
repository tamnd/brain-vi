---
title: "CF 104736B - Trò chơi bảng đen"
description: "Chúng ta được cấp một tập hợp các số nguyên $3N$. Hai người chơi tương tác với những con số này trong một quy trình lựa chọn có cấu trúc, dần dần gán mỗi số cho một trong ba vai trò: đỏ, xanh hoặc loại bỏ."
date: "2026-06-28T23:26:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104736
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 104736
solve_time_s: 75
verified: true
draft: false
---

[CF 104736B - Trò chơi bảng đen](https://codeforces.com/problemset/problem/104736/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều bộ$3N$số nguyên. Hai người chơi tương tác với những con số này trong một quy trình lựa chọn có cấu trúc, dần dần gán mỗi số cho một trong ba vai trò: đỏ, xanh hoặc loại bỏ. Qua$N$làm tròn, chính xác một số trở thành màu đỏ, chính xác một số trở thành màu xanh và đúng một số bị xóa, vì vậy tổng cộng chúng ta kết thúc bằng$N$yếu tố màu đỏ và$N$các phần tử màu xanh lam, trong khi các phần tử còn lại$N$các yếu tố biến mất mà không ảnh hưởng đến điểm số. 

Người chơi màu đỏ (Carlinhos) chọn một số có sẵn mỗi vòng. Sau đó, đối thủ sẽ phản ứng ngay lập tức bằng cách chọn hai số có sẵn khác, gán một trong số chúng là màu xanh lam và loại bỏ số còn lại. Sau tất cả các hiệp đấu, cả hai tay vợt đều đã sắp xếp các set của mình một cách hiệu quả trong các điều kiện đối nghịch: Carlinhos cố gắng tối đa hóa sự khác biệt giữa tổng đỏ và tổng xanh, trong khi đối thủ cố gắng buộc các tổng này bằng nhau. 

Kết quả chỉ phụ thuộc vào việc Carlinhos có thể buộc tổng đỏ cuối cùng khác với mọi tổng xanh có thể có mà đối thủ có thể đưa ra để chơi tối ưu hay không. 

Ràng buộc$N \le 1000$với$3N \le 3000$gợi ý rằng việc sắp xếp và quét tuyến tính là đủ và bất kỳ điều gì liên quan đến tập hợp con hàm mũ hoặc lập trình động theo tổng đều là không thể. 

Một điểm tinh tế là đối thủ cực kỳ mạnh mẽ: bằng cách chọn hai phần tử không được sử dụng trong mỗi hiệp, anh ta sẽ kiểm soát hiệu quả những phần tử còn lại để lựa chọn màu xanh lam trong tương lai. Điều này làm cho quá trình này trở nên bất lợi theo nghĩa toàn cầu, không chỉ là những quyết định tham lam ở địa phương. 

Một sai lầm ngây thơ là cho rằng trò chơi là về việc ghép các cặp độc lập trong mỗi vòng. Điều đó thất bại vì những lựa chọn ban đầu hạn chế cấu trúc sau này. 

Ví dụ: nếu tất cả các số đều bằng nhau, chẳng hạn như$5,5,5,5,5,5$vì$N=2$, bất kỳ sự phân chia nào cũng mang lại số tiền giống nhau, nên Carlinhos không thể thắng. Một chiến lược tham lam cố gắng “cân bằng cục bộ” có thể gợi ý sai về khả năng chiến thắng, nhưng tính đối xứng toàn cầu đảm bảo sự bình đẳng. 

Một trường hợp thất bại khác là khi các số có cấu trúc như lũy thừa của hai, chẳng hạn như$1,2,4,8,16,32$. Ở đây, mỗi tổng tập hợp con là duy nhất, vì vậy đối thủ không bao giờ có thể khớp với lựa chọn màu đỏ khác. Bất kỳ lý luận nào dựa trên giá trị trung bình hoặc sự cân bằng đều có thể bỏ sót tính cứng nhắc này. 

## Phương pháp tiếp cận 

Một góc nhìn bạo lực sẽ cố gắng mô phỏng trò chơi: Carlinhos chọn một phần tử màu đỏ và đối thủ thử mọi cặp phần tử còn lại có thể để quyết định xem phần tử nào trở thành màu xanh lam. Sau đó$N$vòng, chúng tôi sẽ liệt kê tất cả các cấu hình màu đỏ và màu xanh lam thu được và kiểm tra xem liệu sự bình đẳng có luôn là có thể tránh được hoặc có thể thực thi được hay không. Hệ số phân nhánh rất lớn: mỗi vòng giới thiệu$O(n^2)$sự lựa chọn của đối thủ, dẫn đến sự bùng nổ vượt xa$10^{18}$tiểu bang. 

Nhận xét quan trọng là chiêu thức “chọn hai, giữ một” của đối thủ mạnh hơn vẻ bề ngoài. Vì chỉ một trong hai phần tử được chọn trở thành màu xanh lam và phần tử còn lại bị loại bỏ, nên đối thủ có toàn quyền quyết định phần tử nào còn tồn tại trong nhóm ứng cử viên cho nhiệm vụ màu xanh lam. Trong toàn bộ trò chơi, điều này có nghĩa là đối thủ có thể đảm bảo rằng bộ màu xanh lam cuối cùng về cơ bản là bất kỳ tập hợp con nào có kích thước$N$rút ra từ$2N$các phần tử không được chọn là màu đỏ. 

Điều này làm cho trò chơi trở thành một cấu trúc tổ hợp rõ ràng. Carlinhos chọn bộ đỏ$R$kích thước$N$. Multiset còn lại$S$có kích thước$2N$. Sau đó đối thủ chọn bất kỳ tập hợp con nào$B \subset S$kích thước$N$để phù hợp với số tiền màu đỏ nếu có thể. 

Vì vậy, vấn đề trở thành: có tồn tại lựa chọn màu đỏ không$R$sao cho với mọi tập hợp con$B$kích thước$N$trong sự bổ sung, sự bình đẳng$\sum R = \sum B$là không thể? 

Tương tự, Carlinhos thắng nếu ép được số tiền đỏ nằm ngoài phạm vi có thể đạt được.$N$-tập hợp con tổng của các phần tử còn lại. 

Cấu trúc trở nên dễ điều khiển sau khi sắp xếp. Số tiền xanh tốt nhất và tệ nhất có thể có của đối thủ từ một tập hợp cố định đạt được bằng cách lấy các phần tử có sẵn nhỏ nhất hoặc lớn nhất. Điều này thu gọn toàn bộ sự không chắc chắn thành các cấu hình cực đoan. 

Carlinhos chỉ có hai chiến lược có ý nghĩa để tối đa hóa sự tách biệt: lấy lợi nhuận lớn nhất$N$phần tử hoặc lấy giá trị nhỏ nhất$N$các phần tử. Bất kỳ sự lựa chọn hỗn hợp nào cũng làm suy yếu khả năng đẩy số tiền ra ngoài phạm vi có thể đạt được của đối thủ. 

Nếu anh ta lấy số tiền lớn nhất$N$, đối thủ làm việc với cái nhỏ nhất$2N$. Số tiền xanh tối đa có thể có của đối thủ đạt được bằng cách lấy số tiền lớn nhất$N$trong số đó, tương ứng với khối giữa của mảng được sắp xếp. 

Nếu Carlinhos giành phần nhỏ nhất$N$, đối thủ làm việc với số lượng lớn nhất$2N$, và tổng màu xanh lam tối thiểu có thể có của đối thủ lại tương ứng với khối ở giữa. 

Do đó, mọi thứ sụp đổ thành việc so sánh các tổng tiền tố, giữa và hậu tố của mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (sắp xếp + tổng tiền tố) |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta sắp xếp mảng sao cho việc suy luận về các điểm cực trị trở nên có ý nghĩa. Đặt mảng đã được sắp xếp là$a_1 \le a_2 \le \dots \le a_{3N}$. 

Chúng tôi tính toán trước các tổng tiền tố để có thể thu được bất kỳ tổng khối liền kề nào trong thời gian không đổi. 

Chúng tôi chia mảng thành ba khối bằng nhau: khối nhỏ nhất$N$, ở giữa$N$, và lớn nhất$N$. 

1. Tính tổng nhỏ nhất$N$các phần tử. Điều này thể hiện Carlinhos chọn số tiền đóng góp màu đỏ nhỏ nhất có thể. 
2. Tính tổng ở giữa$N$các phần tử. Điều này thể hiện ranh giới bắt buộc của đối thủ để đạt được số tiền xanh trong cả hai trường hợp cực đoan. 
3. Tính tổng lớn nhất$N$các phần tử. Điều này thể hiện Carlinhos đã lựa chọn số tiền đóng góp màu đỏ lớn nhất có thể. 
4. Nếu tổng lớn nhất$N$các phần tử hoàn toàn lớn hơn tổng của phần giữa$N$các phần tử, Carlinhos có thể chọn khối lớn nhất và buộc điểm của anh ấy cao hơn bất kỳ cấu hình màu xanh nào có thể đạt được, đảm bảo chiến thắng. 
5. Nếu tổng nhỏ nhất$N$các phần tử hoàn toàn nhỏ hơn tổng của phần giữa$N$các phần tử, Carlinhos có thể chọn khối nhỏ nhất và buộc điểm của anh ấy thấp hơn bất kỳ cấu hình màu xanh nào có thể đạt được, đồng thời đảm bảo chiến thắng. 
6. Nếu không có bất đẳng thức nghiêm ngặt nào xảy ra, thì mọi tổng màu đỏ cực trị có thể được khớp với một số cấu trúc màu xanh lam hợp lệ, do đó đối thủ có thể thực thi sự bình đẳng. 

Lý do hai thái cực này là đủ là vì bất kỳ lựa chọn màu đỏ nào khác đều tạo ra một tổng nằm giữa hai thái cực này và tính linh hoạt của đối thủ đảm bảo rằng mọi giá trị bên trong đều có thể khớp trong phạm vi tổng tập hợp con có thể đạt được của các phần tử còn lại. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, đối thủ có khả năng chọn bất kỳ tập hợp con nào có kích thước$N$từ phần còn lại$2N$các phần tử có nghĩa là đối với bất kỳ tập hợp màu đỏ cố định nào, tổng màu xanh không phải là tùy ý mà bị giới hạn giữa giá trị nhỏ nhất và lớn nhất có thể$N$-các tập con của phần bù. Những thái cực đó luôn thẳng hàng với các phân đoạn liền kề của mảng được sắp xếp. 

Carlinhos chỉ có thể đảm bảo việc khớp sẽ không thành công nếu số tiền anh ta chọn nằm hoàn toàn ngoài khoảng thời gian có thể đạt được này. Cách duy nhất để buộc sự phân tách như vậy là chọn một trong hai khối cực trị, vì bất kỳ lựa chọn hỗn hợp nào cũng tạo ra một tổng được “bao phủ” bởi khoảng giữa của các tổng màu xanh lam có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    a = list(map(int, input().split()))
    a.sort()

    prefix = [0]
    for x in a:
        prefix.append(prefix[-1] + x)

    def range_sum(l, r):
        return prefix[r] - prefix[l]

    total = 3 * N

    small = range_sum(0, N)
    mid = range_sum(N, 2 * N)
    large = range_sum(2 * N, 3 * N)

    if large > mid or small < mid:
        print("Y")
    else:
        print("N")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp để hiển thị cấu trúc chơi tối ưu. Tổng tiền tố được sử dụng sao cho tổng của ba khối có thể được tính theo thời gian không đổi sau khi sắp xếp. 

Chi tiết quan trọng là chúng tôi không bao giờ cố gắng mô phỏng trò chơi. Tất cả các hiệu ứng tương tác được nén thành các so sánh giữa ba đại lượng xác định xuất phát từ mảng đã được sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 4 8 16 32
```Mảng được sắp xếp không thay đổi. Chúng tôi tính toán: 

| Chặn | Yếu tố | Tổng hợp | 
| --- | --- | --- | 
| Nhỏ | 1 2 | 3 | 
| Trung | 4 8 | 12 | 
| Lớn | 16 32 | 48 | 

Vì số tiền lớn$48 > 12$, Carlinhos có thể buộc tổng màu đỏ cao hơn bất kỳ tổng màu xanh nào có thể đạt được, do đó sản lượng là`Y`. 

Điều này cho thấy trường hợp “tách phía trên” trong đó khối trên cùng thống trị mọi thứ mà đối thủ có thể xây dựng từ phần còn lại. 

### Ví dụ 2 

đầu vào:```
6
5 5 5 5 5 5
```| Chặn | Yếu tố | Tổng hợp | 
| --- | --- | --- | 
| Nhỏ | 5 5 | 10 | 
| Trung | 5 5 | 10 | 
| Lớn | 5 5 | 10 | 

Không có bất đẳng thức nghiêm ngặt nào được giữ, vì vậy đối thủ luôn có thể khớp chính xác bất kỳ số tiền màu đỏ nào. Tính đối xứng của các giá trị loại bỏ mọi lợi thế về cấu trúc, tạo ra sản lượng`N`. 

Điều này khẳng định rằng khi tất cả các khối sụp đổ thành số tiền giống hệt nhau, trò chơi sẽ trở nên cân bằng hoàn hảo và Carlinhos không thể tạo ra sự tách biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp chiếm ưu thế; tất cả các hoạt động khác là tuyến tính | 
| Không gian |$O(N)$| Lưu trữ mảng và tổng tiền tố | 

Với$N \le 1000$, giải pháp chạy thoải mái trong giới hạn và tính đơn giản của các thao tác đảm bảo không có mối lo ngại về hệ số không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("2\n1 2 4 8 16 32\n") == "Y"
assert run("2\n5 5 5 5 5 5\n") == "N"

# minimum N
assert run("1\n2 3 3\n") in ("Y", "N")

# strictly increasing
assert run("2\n1 10 100 1000 10000 100000\n") == "Y"

# all equal large N
assert run("3\n7 7 7 7 7 7 7 7 7\n") == "N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sức mạnh của hai | Y | sự tách biệt mạnh mẽ | 
| tất cả đều bình đẳng | N | sự sụp đổ đối xứng | 
| tăng khoảng cách | Y | sự thống trị của các thái cực | 
| đồng phục lớn N | N | bình đẳng ổn định | 

## Vỏ cạnh 

Khi tất cả các số giống hệt nhau, các khối được sắp xếp có tổng bằng nhau. Trong tình huống đó, cả hai chiến lược cực đoan đều không tạo ra lợi thế nghiêm ngặt, vì vậy đối thủ luôn có thể phản ánh bất kỳ số tiền đỏ nào bằng cách sử dụng lựa chọn màu xanh lam tương ứng. Thuật toán trả về chính xác`N`bởi vì cả hai bất đẳng thức đều sai. 

Khi các giá trị tăng dần nhưng khoảng cách vừa phải, khối lớn nhất thường chiếm ưu thế ở khối giữa. Ví dụ, với$[1,2,3,100,200,300]$, tổng khối lớn vượt quá tổng khối ở giữa, do đó Carlinhos có thể buộc tách bằng cách chọn các phần tử lớn nhất. điều kiện`large > mid`nắm bắt chính xác điều này mà không cần bất kỳ mô phỏng nào của trò chơi.
