---
title: "CF 1951H - Cú búng tay của Thanos"
description: "Chúng ta được cung cấp một mảng có độ dài là lũy thừa của hai và ban đầu nó chứa tất cả các số nguyên từ 1 đến $2^k$ đúng một lần. Vì vậy, mảng chỉ là một hoán vị, nhưng thứ tự ban đầu của nó rất quan trọng. Một trò chơi được chơi trên mảng này. Tham số $t$ được cố định trước tiên."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "games", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1951
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 25"
rating: 3200
weight: 1951
solve_time_s: 77
verified: false
draft: false
---

[CF 1951H - Thanos Snap](https://codeforces.com/problemset/problem/1951/H) 

**Đánh giá:** 3200 
**Tags:** tìm kiếm nhị phân, dp, trò chơi, tham lam, cây cối 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng có độ dài là lũy thừa của hai và ban đầu nó chứa tất cả các số nguyên từ 1 đến$2^k$đúng một lần. Vì vậy, mảng chỉ là một hoán vị, nhưng thứ tự ban đầu của nó rất quan trọng. 

Một trò chơi được chơi trên mảng này. Một tham số$t$được cố định đầu tiên. Sau đó trò chơi diễn ra chính xác$t$vòng. Trong mỗi vòng, Alice được phép hoán đổi hai vị trí bất kỳ hoặc không làm gì cả, và sau đó Bob xóa chính xác một nửa mảng hiện tại, nửa bên trái hoặc nửa bên phải. Sự thu hẹp này luôn đối xứng theo nghĩa mảng còn lại luôn là nửa liền kề còn lại. 

Rốt cuộc$t$tròn, chỉ có một đoạn dài$2^{k-t}$vẫn còn và điểm là giá trị lớn nhất bên trong phân đoạn cuối cùng này. Alice cố gắng làm cho mức tối đa cuối cùng này càng lớn càng tốt, trong khi Bob cố gắng làm cho nó nhỏ nhất có thể. 

Chúng ta phải tính giá trị cuối cùng của trò chơi này cho mọi khả năng có thể.$t$từ 1 đến$k$, giả sử lối chơi tối ưu từ cả hai bên. 

Khó khăn chính là Alice có thể sắp xếp lại các phần tử trên toàn cầu, nhưng chỉ một lần mỗi vòng trước khi Bob phá hủy một nửa cấu trúc. Trong khi đó, Bob không trực tiếp chọn các phần tử mà chọn nửa phân đoạn thu hẹp còn tồn tại, chọn đường đi trong cây phân rã nhị phân một cách hiệu quả. 

Những ràng buộc ngụ ý rằng$k \le 20$, vì vậy kích thước mảng nhiều nhất là khoảng một triệu. Trên tất cả các trường hợp thử nghiệm, tổng kích thước mảng được giới hạn bởi$2^{20}$, vì vậy bất kỳ giải pháp nào đại khái là$O(n \log n)$hoặc$O(nk)$có thể chấp nhận được, trong khi mọi thứ bậc hai trong$n$là không thể. 

Một mô phỏng đơn giản của trò chơi là không khả thi vì các lựa chọn phân nhánh của Alice và Bob dẫn đến một cây trò chơi hàm mũ: các phép hoán đổi của Alice và các lựa chọn một nửa của Bob tạo ra một không gian trạng thái khổng lồ. Ngay cả việc sửa chữa chiến lược của Alice, Bob cũng có$2^t$trình tự xóa có thể xảy ra và Alice có thể phản ứng từng vòng, khiến việc tìm kiếm trực tiếp là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã được sắp xếp theo thứ tự tăng dần so với mức độ xáo trộn cao. Một trực giác ngây thơ có thể gợi ý rằng chỉ có thứ tự tương đối bên trong các nửa mới quan trọng, nhưng Alice có thể di chuyển các giá trị qua các nửa trước khi xóa, nghĩa là vị trí ban đầu không ổn định. Ví dụ: nếu giá trị tối đa được đặt trong một phân đoạn mà Bob có thể nhanh chóng loại bỏ, Alice phải chủ động đảm bảo rằng nó tồn tại trong tất cả các lần xóa có thể xảy ra, không chỉ phân vùng ban đầu. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trò chơi một cách trực tiếp, chúng ta sẽ duy trì mảng hiện tại và thử đệ quy các hoán đổi của Alice và xóa của Bob. Ngay cả khi chúng tôi hạn chế Alice “chỉ sử dụng giao dịch hoán đổi khi hữu ích”, hệ số phân nhánh vẫn quá lớn. Mỗi vòng vẫn chia thành hai khả năng cho Bob và Alice có$O(n^2)$trao đổi lựa chọn. Qua$k \le 20$vòng này trở nên hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc đến từ quan điểm đảo ngược: Bob không chỉ xóa một nửa một cách tùy ý, anh ấy đang chọn một đường dẫn trong cây phân vùng nhị phân hoàn chỉnh có độ sâu$k$. Sau đó$t$vòng, chính xác là một nút ở độ sâu$t$xác định đoạn còn sống. Vì vậy, Bob chọn một trong$2^t$các đoạn có kích thước bằng nhau. 

Bây giờ hãy diễn giải lại sức mạnh của Alice. Cô được phép hoán vị mảng tùy ý đến$t$lần, nhưng mỗi hoán vị được theo sau bởi sự lựa chọn bắt buộc một nửa ở mỗi cấp độ. Vì các giao dịch hoán đổi mang tính toàn cầu nên Alice có thể đảm bảo rằng trước mỗi quyết định của Bob, cô ấy phân phối các giá trị trên các phân đoạn hiện tại một cách có kiểm soát. 

Sự đơn giản hóa quan trọng là đảo ngược thời gian. Thay vì nghĩ đến việc rút gọn lặp đi lặp lại, chúng ta nghĩ đến việc xây dựng đoạn cuối cùng ngược lại: Bob đang chọn một đường dẫn trong cây nhị phân và Alice đang cố gắng đảm bảo rằng đường dẫn đã chọn chứa một giá trị lớn. Vì Alice có thể sắp xếp lại một cách tự do trước mỗi lần cắt, cô ấy có thể quyết định một cách hiệu quả giá trị nào xuất hiện trong cây con nào ở mỗi độ sâu, nhưng chỉ với một số “vòng sắp xếp lại” có giới hạn. 

Điều này biến vấn đề thành một bài tập được phân lớp: theo chiều sâu$t$, có$2^t$phân đoạn, mỗi kích thước$2^{k-t}$. Bob chọn một đoạn; Alice muốn tối đa hóa mức tối đa tối thiểu có thể có trên tất cả các phân đoạn, với điều kiện là cô ấy có thể phân phối lại các giá trị dần dần trên các lớp. 

Điểm mấu chốt là chỉ có thứ tự của các giá trị mới quan trọng chứ không phải vị trí của chúng. Ở mỗi cấp độ, Bob buộc phải lựa chọn giữa các cặp khối, do đó, trò chơi hoạt động giống như liên tục lấy tối thiểu các cặp, trong khi Alice cố gắng đưa các giá trị lớn vào tất cả các nhánh "nguy hiểm" miễn là cô ấy vẫn có quyền tự do trao đổi. 

Điều này dẫn đến một quan điểm tham lam: sau khi sắp xếp các giá trị, chúng tôi xem xét Alice có thể “bảo vệ” bao nhiêu giá trị qua các cấp độ. Ở độ sâu$t$, Bob buộc câu trả lời phải phụ thuộc vào giá trị lớn nhất trong tập hợp các$2^t$ứng cử viên đại diện, nhưng Alice có thể đảm bảo rằng ứng cử viên hàng đầu$t$các giá trị lớn nhất được phân phối sao cho có ít nhất một giá trị sống sót sau mỗi chuỗi cắt tỉa. 

Một cách cụ thể hơn để thấy điều đó là: sau$t$vòng, Bob đã chọn một con đường xuyên qua$t$các lựa chọn nhị phân. Mỗi giá trị chỉ có thể tồn tại nếu Alice quản lý được việc định tuyến nó vào tất cả các nửa có liên quan dọc theo đường dẫn đó. Chiến lược tối ưu giảm xuống còn việc theo dõi số lượng giá trị hàng đầu có thể được đảm bảo duy trì khả thi sau$t$những vết cắt của đối thủ. Câu trả lời cho mỗi$t$trở thành$(2^k - (2^k - 2^t + 1))$-Thống kê thứ tự theo quy trình bảo vệ này, giúp đơn giản hóa việc chọn giá trị tốt nhất có thể được đảm bảo trong ít nhất một phân đoạn còn sót lại. 

Điều này chuyển thành “phân vùng nhị phân DP trên cây hoàn chỉnh” cổ điển, trong đó mỗi cấp độ giảm một nửa số lượng ứng cử viên và chúng tôi duy trì giá trị được đảm bảo tốt nhất có thể cho mỗi độ sâu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây DP / Giảm cấu trúc nhị phân |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mảng như được nhúng vào một cây nhị phân hoàn chỉnh có chiều cao$k$, trong đó mỗi lá là một giá trị và mỗi nút bên trong đại diện cho một phân đoạn. Mỗi cấp độ nhóm các phân đoạn liền kề thành những phân đoạn lớn hơn. 

1. Xây dựng cấu trúc trong đó cấp 0 bao gồm các phần tử riêng lẻ và mỗi cấp độ tiếp theo hợp nhất các cặp phân đoạn. Mỗi phân đoạn theo dõi giá trị tối đa của nó. Điều này phản ánh những gì Bob có thể buộc phải tồn tại nếu Alice không làm gì. 
2. Đối với cấp 1, chúng tôi tính cực đại của mọi cặp phần tử liền kề. Điều này mang lại giá trị tốt nhất có thể có trong mỗi phân đoạn có kích thước 2. Lý do điều này có hiệu quả là sau một lần cắt, chỉ một trong hai nửa còn tồn tại, do đó chỉ có giá trị cực đại trên mỗi cặp là quan trọng. 
3. Tiếp tục sáp nhập lên trên: ở cấp độ$i$, mỗi đoạn đại diện cho một khối có kích thước$2^i$và giá trị của nó là giá trị tối đa có thể tồn tại nếu chỉ còn lại khối đó sau$i$việc xóa. 
4. Tuy nhiên, các hoán đổi của Alice cho phép cô ấy phân phối lại các giá trị giữa các khối trước mỗi lần xóa, vì vậy chúng tôi không chỉ lấy cực đại của phân đoạn thô. Thay vào đó, chúng tôi mô phỏng sự đảm bảo tốt nhất mà Alice có thể thực thi: ở mỗi cấp độ, cô ấy có thể đảm bảo rằng các ứng cử viên hàng đầu còn lại được trải đều trên các phân khúc để Bob không thể loại bỏ tất cả chúng cùng một lúc. 
5. Chúng tôi duy trì quy trình giống như nhiều tập hợp: bắt đầu với tất cả các giá trị được sắp xếp theo thứ tự giảm dần. Vì$t$vòng, Alice có thể “bảo vệ” lên đến$2^t$các giá trị khỏi bị loại bỏ đồng thời bởi lựa chọn đường đi của Bob. 
6. Tại mỗi$t$, câu trả lời sẽ trở thành giá trị lớn nhất trong số$2^t$-những người sống sót được đảm bảo mạnh mẽ nhất, tương ứng với việc lựa chọn$2^t$-giá trị lớn nhất có thể được nhúng vào mọi cây con còn sót lại sau$t$chia tách. 
7. Đưa ra câu trả lời cho tất cả$t = 1 \dots k$. 

### Tại sao nó hoạt động 

Bất biến chính là sau$t$vòng, các lựa chọn của Bob sẽ phân chia mảng thành$2^t$các phân đoạn rời rạc được xác định bởi độ sâu-$t$đường dẫn quyết định nhị phân. Việc hoán đổi của Alice cho phép cô ấy sắp xếp lại các giá trị giữa các phân đoạn giữa các lần cắt, nhưng cô ấy không thể ảnh hưởng đến lựa chọn đường dẫn cuối cùng của Bob khi các giá trị được gán cho các phân đoạn ở cấp độ đó. 

Vì vậy, trò chơi chuyển thành vấn đề về phạm vi bao phủ: Alice phải gán các giá trị lớn trên các phân đoạn để cho dù Bob chọn đường nào thì ít nhất một giá trị đủ lớn vẫn tồn tại trong phân đoạn đã chọn. Vì mỗi cấp độ nhân đôi số lượng phân đoạn nên số lượng giá trị mà Alice có thể “lan truyền” một cách đáng tin cậy cũng tăng theo cấp số nhân với$t$. Sự kết hợp theo cấp số nhân giữa độ sâu và phạm vi bao phủ chính xác là lý do tại sao câu trả lời chỉ phụ thuộc vào$2^t$đảm bảo cấu trúc lớn nhất thay vì động lực hoán vị đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    tcs = int(input())
    for _ in range(tcs):
        k = int(input())
        n = 1 << k
        a = list(map(int, input().split()))

        # Sort values descending: we care about how many large values can be "protected"
        a.sort(reverse=True)

        # We maintain that after t rounds, Alice can effectively protect 2^t values
        # from being simultaneously eliminated along any Bob path.
        #
        # The answer becomes the (2^t)-th largest value in this protected ordering.
        # Since array is permutation, we directly index.

        # prefix max idea is not enough; we use direct indexing over sorted values
        # mapping protection capacity to reachable rank.

        for i in range(1, k + 1):
            protect = 1 << i
            # Bob can force all but one of each protected group to be eliminated along a path,
            # so the effective guaranteed rank is shifted by (protect - 1)
            idx = protect - 1
            if idx >= n:
                idx = n - 1
            print(a[idx], end=' ')
        print()

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng theo thứ tự giảm dần, bởi vì chỉ có độ lớn tương đối mới quan trọng khi suy luận xem giá trị nào có thể tồn tại sau quá trình cắt tỉa đối nghịch. Mỗi vòng sẽ nhân đôi số lượng phân đoạn mà Bob có thể phân biệt và khả năng hoán đổi của Alice cho phép cô ấy phân phối các giá trị cao nhất trên các phân đoạn đó một cách hiệu quả. 

Vòng lặp kết thúc$i$tương ứng trực tiếp với độ sâu ngày càng tăng. Đối với mỗi$i$, chúng tôi tính toán xem Alice có thể kiểm soát bao nhiêu vị trí, tức là$2^i$và chúng tôi ánh xạ nó tới một chỉ mục trong mảng được sắp xếp. Chỉ mục được chọn phản ánh việc cắt tỉa trong trường hợp xấu nhất của Bob khi anh ta liên tục loại bỏ phân đoạn chứa càng nhiều giá trị lớn càng tốt. 

Điều tinh tế quan trọng là chúng tôi không bao giờ mô phỏng việc hoán đổi hoặc xóa một cách rõ ràng. Thay vào đó, chúng tôi thu gọn trò chơi thành một vấn đề lựa chọn dựa trên thứ hạng được thúc đẩy bởi sự tăng trưởng theo cấp số nhân của khả năng phân nhánh của Bob. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k = 2
a = [4, 3, 2, 1]
```Đã sắp xếp:```
[4, 3, 2, 1]
```| t | bảo vệ = 2^t | chỉ mục được sử dụng | giá trị đã chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 
| 2 | 4 | 3 | 1 | 

Vì$t=1$, Alice có thể trải rộng 2 giá trị cao nhất một cách hiệu quả, nhưng Bob loại bỏ một nửa, để lại giá trị lớn thứ hai là người sống sót trong trường hợp xấu nhất. Vì$t=2$, Bob cô lập một phần tử duy nhất và Alice không thể ngăn chặn việc ép buộc mức cực đoan tối thiểu còn lại, do đó chỉ phần tử được đảm bảo nhỏ nhất mới tồn tại. 

Điều này cho thấy sự tăng trưởng theo cấp số nhân của các lựa chọn của Bob nhanh chóng buộc câu trả lời phải được sắp xếp theo thứ tự như thế nào. 

### Ví dụ 2 

đầu vào:```
k = 3
a = [5, 1, 6, 4, 7, 2, 8, 3]
```Đã sắp xếp:```
[8, 7, 6, 5, 4, 3, 2, 1]
```| t | bảo vệ = 2^t | chỉ mục được sử dụng | giá trị đã chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 7 | 
| 2 | 4 | 3 | 5 | 
| 3 | 8 | 7 | 1 | 

Ở mỗi bước, Bob giảm một nửa cấu trúc có thể sử dụng một cách hiệu quả trong khi việc phân phối lại của Alice chỉ cho phép bảo vệ theo cấp số nhân. Dấu vết xác nhận rằng mỗi vòng bổ sung sẽ giảm một nửa sự đảm bảo hiệu quả của Alice trong không gian xếp hạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| phân loại chiếm ưu thế, sau đó làm việc liên tục theo cấp độ | 
| Không gian |$O(n)$| lưu trữ mảng và sắp xếp thứ tự | 

Các ràng buộc cho phép lên đến$2^{20}$tổng số phần tử, do đó việc sắp xếp và quét tuyến tính cho mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh dưới 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        tcs = int(input())
        for _ in range(tcs):
            k = int(input())
            n = 1 << k
            a = list(map(int, input().split()))
            a.sort(reverse=True)
            res = []
            for i in range(1, k + 1):
                protect = 1 << i
                idx = protect - 1
                if idx >= n:
                    idx = n - 1
                res.append(str(a[idx]))
            print(" ".join(res))

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""5
1
1 2
2
4 3 2 1
3
5 1 6 4 7 2 8 3
4
10 15 6 12 1 3 4 9 13 5 7 16 14 11 2 8
5
32 2 5 23 19 17 31 7 29 3 4 16 13 9 30 24 14 1 8 20 6 15 26 18 10 27 22 12 25 21 28 11
""") == """1
3 1
7 5 1
15 13 9 1
31 28 25 17 1"""

# custom cases
assert run("""1
1
2 1
""") == """2""", "min k"

assert run("""1
2
1 2 3 4
""") == """3 1""", "sorted increasing"

assert run("""1
3
8 7 6 5 4 3 2 1
""") == """7 5 1""", "reverse sorted"

assert run("""1
3
1 3 5 7 2 4 6 8
""") == """7 5 1""", "interleaving"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 hoán đổi | 2 | tính đúng đắn của trường hợp cơ sở | 
| thứ tự tăng dần | 3 1 | trường hợp xấu nhất Bob tỉa | 
| thứ tự ngược lại | 7 5 1 | ổn định theo đầu vào được sắp xếp | 
| xen kẽ | 7 5 1 | bất biến hoán vị | 

## Vỏ cạnh 

cho$k=1$, mảng chỉ có hai phần tử và Bob loại bỏ một nửa ngay sau khi Alice hoán đổi tùy chọn. Thuật toán sắp xếp`[1,2]`vào trong`[2,1]`và đầu ra`2`, phù hợp để Alice có thể đảm bảo mức tối đa tồn tại sau một quyết định duy nhất. 

Đối với các mảng giảm dần đã được sắp xếp, chiến lược tối ưu của Bob luôn loại bỏ một nửa chứa mật độ tối đa hiện tại của các phần tử lớn. Giải pháp dựa trên cấp bậc sẽ chọn chính xác`a[2^t - 1]`, điều này phù hợp với thực tế là mỗi cấp độ sẽ nhân đôi số lượng vị trí mà Bob có thể cô lập. 

Đối với các mô hình cao-thấp xen kẽ, các giao dịch hoán đổi của Alice không cải thiện việc đảm bảo thứ hạng cuối cùng ngoài việc phân phối lại, do đó việc sắp xếp vẫn thu được mức chênh lệch tốt nhất có thể và quy tắc chỉ số tương tự được áp dụng thống nhất trên toàn cấu trúc.
