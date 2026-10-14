---
title: "CF 105182I - Trò chơi số"
description: "Chúng ta được cấp một tập hợp các số nguyên dương biểu thị các trọng số được đặt trong một chuỗi. Hai người chơi luân phiên nhau và trong mỗi lượt, họ nén chuỗi bằng cách hợp nhất hai phần tử đã chọn thành tổng của chúng hoặc họ ngay lập tức kết thúc trò chơi bằng cách chọn một phần tử."
date: "2026-06-27T04:40:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105182
codeforces_index: "I"
codeforces_contest_name: "The 22nd UESTC Programming Contest - Final"
rating: 0
weight: 105182
solve_time_s: 63
verified: true
draft: false
---

[CF 105182I - Trò chơi số](https://codeforces.com/problemset/problem/105182/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương biểu thị các trọng số được đặt trong một chuỗi. Hai người chơi luân phiên nhau và trong mỗi lượt, họ nén chuỗi bằng cách hợp nhất hai phần tử đã chọn thành tổng của chúng hoặc họ ngay lập tức kết thúc trò chơi bằng cách chọn một phần tử. Khi người chơi chọn một số, người chơi đó sẽ nhận được giá trị của nó và đối thủ nhận được tổng của tất cả các phần tử còn lại. Trò chơi sau đó dừng lại. 

Tổng số của tất cả các số không bao giờ thay đổi trong suốt trò chơi trừ khi trò chơi kết thúc. Cách duy nhất để tác động đến kết quả cuối cùng là hợp nhất các con số, điều này sẽ định hình lại cách phân bổ tổng số tiền cho các lựa chọn trong tương lai. 

Quyết định cuối cùng trong bất kỳ nước đi kết thúc nào nhìn chung đều cực kỳ đơn giản. Nếu một người chơi lấy giá trị x từ tổng số tiền hiện tại là S, thì đối thủ nhận được S − x, do đó người lấy điểm sẽ thắng chính xác khi 2x > S. 

Do đó, toàn bộ vấn đề là liệu người chơi đầu tiên có thể buộc một tình huống mà họ là người chọn một giá trị đủ lớn so với tổng số còn lại hay không, mặc dù đối thủ cũng có thể hợp nhất và có khả năng kết thúc trò chơi sớm hơn. 

Các ràng buộc ngụ ý rằng việc mô phỏng trực tiếp các trạng thái trò chơi là không thể. Với tối đa 10^5 trường hợp thử nghiệm và tổng kích thước mảng lên tới 10^6, mọi tìm kiếm trong không gian trạng thái trên tất cả lịch sử hợp nhất hoặc cây trò chơi sẽ vượt xa giới hạn khả thi. Ngay cả việc duy trì các trạng thái trò chơi động sẽ đưa ra ít nhất hành vi bậc hai vì mỗi lần hợp nhất sẽ giảm kích thước chuỗi đi một nhưng hệ số phân nhánh của các lựa chọn là lớn. 

Một sai lầm ngây thơ là coi đây là một trò chơi “lấy phần tử tối đa” đơn giản. Điều đó không thành công vì việc hợp nhất có thể tạo ra các giá trị mới không xuất hiện ban đầu và những giá trị mới đó có thể mang tính quyết định trong so sánh cuối cùng với tổng còn lại. 

Một chế độ thất bại khác là giả sử chơi tham lam, chẳng hạn như luôn hợp nhất hai giá trị nhỏ nhất. Điều đó bỏ qua khả năng phản ứng của đối thủ bằng cách kết thúc ván đấu sớm khi xuất hiện thế mất cân bằng có lợi. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực của trò chơi là coi mọi trạng thái là một tập hợp nhiều tập hợp và mô phỏng cả hai người chơi khám phá tất cả các nước đi hợp lệ. Mỗi lần hợp nhất sẽ giảm số lượng phần tử đi một phần tử và mỗi bước kết thúc sẽ đánh giá sự phân chia điểm cuối cùng. Điều này dẫn đến số lượng trạng thái trò chơi tăng theo cấp số nhân vì ở mỗi bước, người chơi chọn bất kỳ cặp nào để hợp nhất hoặc bất kỳ yếu tố nào để kết thúc trò chơi. Ngay cả khi bỏ qua việc chấm dứt, số lượng lịch sử hợp nhất có thể tương ứng với số lượng cây nhị phân trên n lá, có kích thước bằng Catalan và phát triển theo cấp số nhân trong thực tế. 

Quan sát quan trọng là việc hợp nhất không làm thay đổi tổng mà chỉ thay đổi cách nhóm tổng. Bất kỳ chuỗi hợp nhất nào cũng tương đương với việc xây dựng một cây nhị phân đầy đủ trên các phần tử ban đầu, trong đó mỗi nút bên trong biểu thị tổng của một tập hợp con. Do đó, trò chơi tập trung vào việc kiểm soát cách hình thành phân vùng cuối cùng của mảng khi chỉ còn lại hai nhóm tổng hợp. 

Thay vì theo dõi các trạng thái của trò chơi, chúng ta có thể suy luận về cấu trúc cuối cùng không thể tránh khỏi. Cuối cùng, cách chơi tối ưu sẽ giảm chuỗi xuống còn hai số, bởi vì việc dừng sớm hơn số đó bị chi phối bởi việc tiếp tục hợp nhất: kết thúc trò chơi quá sớm sẽ mang lại cho đối thủ một số tiền còn lại có thể lớn. Ở hai phần tử, không có sự hợp nhất an toàn nào nữa, bởi vì việc hợp nhất sẽ ngay lập tức cho phép đối thủ lấy giá trị duy nhất còn lại và giành chiến thắng với tổng số tiền đầy đủ. 

Vì vậy, toàn bộ trò chơi chuyển sang câu hỏi liệu người chơi đầu tiên có thể buộc điều đó hay không, tại thời điểm trình tự giảm xuống còn hai nhóm, họ có thể chọn một nhóm có giá trị vượt quá một nửa tổng số tiền.

Điều này biến trò chơi thành một vấn đề kiểm soát cách thức phân chia cuối cùng được hình thành. Mỗi hoạt động hợp nhất đang quyết định một cách hiệu quả cách các phần tử được nhóm lại và cả hai người chơi đang định hình cây phân vùng nhị phân với các lựa chọn đối nghịch. 

Cái nhìn sâu sắc mang tính quyết định là bất chấp lối chơi đối kháng, cấu trúc của vấn đề vẫn sụp đổ thành một bất biến đơn giản: cách duy nhất mà người chơi đầu tiên có thể đảm bảo chiến thắng là nếu tồn tại một phần tử có giá trị lớn hơn tổng của tất cả các phần tử khác. Nếu một yếu tố như vậy tồn tại, người chơi đầu tiên có thể buộc nó vẫn đủ “cô lập” thông qua việc hợp nhất và cuối cùng đảm bảo nó là lựa chọn cuối cùng trước khi đối thủ có thể vô hiệu hóa nó. 

Nếu không có yếu tố chi phối như vậy tồn tại, đối thủ luôn có thể đáp trả bằng cách buộc phải chia đôi cuối cùng để tránh đưa ra lựa chọn chiến thắng cho người chơi đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây trò chơi Brute Force | O(số mũ) | O(tiểu bang) | Quá chậm | 
| Bất biến tham lam tối ưu | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp giảm xuống còn việc kiểm tra xem phần tử lớn nhất có thể thống trị phần còn lại của mảng hay không. 

1. Tính tổng S của tất cả các phần tử trong dãy. Giá trị này vẫn bất biến khi hợp nhất, vì vậy nó là tham chiếu để đánh giá mọi quyết định cuối cùng. 
2. Xác định phần tử lớn nhất mx trong mảng. Bất kỳ chiến lược chiến thắng nào cuối cùng đều phải dựa vào việc buộc giá trị này phải được chọn vào đúng thời điểm, vì không có sự kết hợp nào của các yếu tố nhỏ hơn có thể vượt quá giá trị đó nếu nó đã chiếm ưu thế trên toàn cầu. 
3. So sánh mx với S − mx. Nếu mx hoàn toàn lớn hơn S − mx thì mx đủ mạnh để nếu nó được chọn làm lượt cuối cùng thì người chơi được chọn sẽ thắng ngay lập tức. 
4. Nếu điều kiện này được giữ, hãy kết luận rằng người chơi đầu tiên có thể thực thi một chuỗi các phép hợp nhất để duy trì khả năng chọn mx vào đúng thời điểm, dẫn đến chiến thắng bắt buộc. 
5. Nếu không, hãy kết luận rằng đối thủ luôn có thể phản ứng theo cách ngăn cản người chơi đầu tiên đạt được lựa chọn cuối cùng chiến thắng. 

Ý tưởng chính là việc hợp nhất chỉ phân phối lại trọng số và không có chuỗi phân phối lại nào có thể tạo ra phần tử mới mạnh hơn mức tối đa toàn cục. Vì vậy, về cơ bản, toàn bộ trò chơi được kiểm soát bằng việc liệu một yếu tố duy nhất đã chiếm ưu thế trong tổng hay chưa. 

### Tại sao nó hoạt động 

Mọi nước đi đều bảo toàn tổng số S và mọi nước đi cuối cùng được quyết định bằng cách so sánh một giá trị x đã chọn duy nhất với S − x. Vì việc hợp nhất chỉ thay thế hai giá trị bằng tổng của chúng nên chúng không bao giờ tăng “lợi thế có thể lựa chọn cuối cùng” riêng lẻ tối đa có thể vượt quá những gì đã có trong một số tập hợp con. Cách duy nhất để đảm bảo nước đi cuối cùng chiến thắng là đảm bảo rằng tồn tại một giá trị có thể tách biệt dưới dạng phép chọn thỏa mãn 2x > S. Nếu ban đầu không có giá trị như vậy tồn tại thì không có chuỗi hợp nhất nào có thể tạo ra nó, bởi vì bất kỳ giá trị hợp nhất nào đều tương ứng với một tập hợp con có tổng phần bù ít nhất bằng lớn, cho phép đối thủ vô hiệu hóa lợi thế ở bước cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a)
        mx = max(a)
        if mx > s - mx:
            out.append("YES")
        else:
            out.append("NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện bất biến xuất phát ở trên. Đối với mỗi trường hợp thử nghiệm, nó tính tổng và phần tử lớn nhất. Quyết định chỉ phụ thuộc vào việc phần tử tối đa có lớn hơn tổng của tất cả các phần tử còn lại hay không. Không cần mô phỏng việc hợp nhất vì việc hợp nhất không ảnh hưởng đến tổng tổng hoặc danh tính của phần tử tối đa. 

Việc triển khai cẩn thận tránh chi phí cho mỗi hoạt động và giữ mọi thứ tuyến tính cho mỗi trường hợp thử nghiệm, điều này cần thiết với kích thước đầu vào tổng hợp lên tới một triệu phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 3]
```| Bước | Tổng S | Tối đa mx | mx vs S - mx | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | 3 | 3 vs 3 | KHÔNG | 

Phần tử lớn nhất bằng một nửa tổng nên không chiếm ưu thế hoàn toàn. Bất kỳ nỗ lực nào để cô lập nó vẫn khiến đối thủ có tổng trọng lượng bằng nhau, vì vậy người chơi đầu tiên không thể buộc phải chọn chiến thắng cuối cùng một cách nghiêm ngặt. 

Điều này chứng tỏ trường hợp ranh giới trong đó sự bình đẳng là không đủ. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [1, 2, 6]
```| Bước | Tổng S | Tối đa mx | mx vs S - mx | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 9 | 6 | 6 đấu 3 | CÓ | 

Ở đây phần tử tối đa thống trị tất cả các phần tử còn lại cộng lại. Điều này đảm bảo rằng nếu trò chơi đạt đến trạng thái có thể lấy được yếu tố này, người chơi được chọn sẽ ngay lập tức chiến thắng. Người chơi đầu tiên có thể chỉ đạo việc hợp nhất để duy trì sự thống trị này. 

Điều này xác nhận trực giác rằng một yếu tố cực kỳ lớn sẽ quyết định kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được đọc một lần để tính tổng và giá trị lớn nhất | 
| Không gian | O(1) thêm | Chỉ các biến tổng hợp được duy trì | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 10^6, do đó, một lần truyền tuyến tính duy nhất trên tất cả các giá trị dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            s = sum(a)
            mx = max(a)
            out.append("YES" if mx > s - mx else "NO")
        print("\n".join(out))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return res

# provided sample (as parsed)
assert run("3\n3\n1 2 3\n3\n1 2 6\n3\n4 6 2\n") == "NO\nYES\nNO"

# all equal values
assert run("1\n4\n2 2 2 2\n") == "NO"

# single large dominant element
assert run("1\n5\n1 1 1 1 10\n") == "YES"

# minimum size
assert run("1\n1\n5\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | KHÔNG | không yếu tố nào có thể thống trị | 
| chiếm ưu thế tối đa | CÓ | điều kiện thống trị nghiêm ngặt | 
| phần tử đơn | CÓ | trường hợp thắng tầm thường | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng nhau, mọi giá trị ứng cử viên bằng chính xác một nửa tổng chỉ trong trường hợp suy biến n=2 và hoàn toàn nhỏ hơn trong trường hợp khác. Thuật toán đưa ra NO chính xác vì không có phần tử nào thỏa mãn sự thống trị nghiêm ngặt. 

Khi n bằng 1 thì số lớn nhất bằng tổng, do đó điều kiện đúng và câu trả lời là CÓ. Người chơi ngay lập tức lấy yếu tố duy nhất và giành chiến thắng. 

Khi có một phần tử ngoại lệ rất lớn, việc hợp nhất không thể làm giảm giá trị của nó hoặc phân phối lại lợi thế của nó so với so sánh cuối cùng. Thuật toán xác định chính xác điều này và trả về CÓ.
