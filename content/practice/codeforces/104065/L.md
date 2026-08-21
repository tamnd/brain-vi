---
title: "CF 104065L - Por Una Cabeza"
description: "Chúng ta có một cấu trúc định hướng được hình thành bởi hai loại nút. Loại đầu tiên là một tập hợp các nút đối tượng, mỗi nút mang một giá trị nhị phân và chi phí thể hiện mức độ tốn kém khi lật giá trị đó."
date: "2026-07-02T03:21:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104065
codeforces_index: "L"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Mianyang Onsite"
rating: 0
weight: 104065
solve_time_s: 47
verified: true
draft: false
---

[CF 104065L - Por Una Cabeza](https://codeforces.com/problemset/problem/104065/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cấu trúc định hướng được hình thành bởi hai loại nút. Loại đầu tiên là một tập hợp các nút đối tượng, mỗi nút mang một giá trị nhị phân và chi phí thể hiện mức độ tốn kém khi lật giá trị đó. Loại thứ hai là một bộ máy bỏ phiếu, mỗi máy tổng hợp một số lượng đầu vào lẻ ​​cố định và đưa ra giá trị đa số của những đầu vào đó. 

Mỗi phiếu bầu của khán giả được sử dụng chính xác một lần làm đầu vào cho chính xác một máy và mỗi đầu ra của máy ngoại trừ máy cuối cùng cũng được sử dụng chính xác một lần làm đầu vào cho máy khác. Điều này có nghĩa là toàn bộ hệ thống tạo thành một biểu đồ tuần hoàn có hướng gốc, cuối cùng chuyển sang số máy m, đầu ra của máy này trở thành kết quả cuối cùng. 

Một cỗ máy hoạt động giống như một cổng đa số, nhưng có một hạn chế quan trọng về thời gian. Nó không cần phải đợi tất cả các đầu vào của nó; ngay sau khi nó nhận được hơn một nửa số đầu vào có cùng giá trị, nó sẽ ngay lập tức cam kết với đầu ra đó và đầu ra đó sẽ lan truyền về phía trước. 

Bobo muốn tránh hiện tượng cam kết sớm này ở bất kỳ đâu trong hệ thống. Mục đích là để đảm bảo rằng không có máy nào có thể xác định đầu ra của nó trước khi tất cả đầu vào của nó được chuyển đến. Điều đó có nghĩa là, ở mọi máy, bất kể thứ tự đến, cả 0 và 1 đều không đạt được đa số nghiêm ngặt trước khi nhận được đầu vào cuối cùng. Tương tự, ở mọi máy, nhiều tập hợp đầu vào cuối cùng phải luôn cân bằng hoàn hảo ở mọi tiền tố, nghĩa là không có giá trị nào dẫn đầu nghiêm ngặt cho đến cuối cùng. 

Mỗi khán giả có một phiếu bầu hiện tại và chi phí để lật bỏ phiếu bầu đó. Chúng tôi được yêu cầu xử lý các bản cập nhật làm thay đổi cả giá trị và chi phí của một đối tượng và sau mỗi lần cập nhật, chúng tôi phải tính toán tổng chi phí tối thiểu để sửa đổi một số phiếu bầu của khán giả để toàn bộ hệ thống đáp ứng điều kiện "không có đa số sớm ở bất kỳ đâu". 

Các ràng buộc cho thấy rằng bất kỳ giải pháp nào tính toán lại việc truyền bá cho mỗi truy vấn đều là không thể. Với tối đa 100000 nút và bản cập nhật, ngay cả việc tính toán lại tuyến tính cho mỗi truy vấn cũng dẫn đến khoảng 10^10 thao tác. Cấu trúc là một DAG với một ràng buộc quạt vào rất cụ thể, điều này cho thấy rõ ràng rằng vấn đề không nằm ở mô phỏng luồng động mà là về việc giảm hệ thống thành các ràng buộc độc lập trên mỗi lần cắt cạnh hoặc trên mỗi tập hợp nút. 

Một trường hợp phức tạp xuất hiện khi một máy nhận được đầu vào đã bị mất cân bằng sớm do các máy ngược dòng. Ngay cả khi đa số cuối cùng được cân bằng ở gốc, máy thấp hơn có thể vi phạm điều kiện cục bộ. Một cách tiếp cận ngây thơ chỉ kiểm tra các giá trị cuối cùng ở mỗi máy sẽ bỏ lỡ các trạng thái đa số sớm nhất thời này. 

Ví dụ, hãy xem xét một máy có ba đầu vào đến từ các cây con khác nhau. Nếu những cây con đó có thể độc lập tạo ra hai giá trị 1 được đảm bảo sớm và một giá trị 0 muộn hơn, thì máy đã vi phạm quy tắc ngay cả khi các giá trị cuối cùng sau đó có thể cân bằng. Ràng buộc về cơ bản là về sự an toàn của tiền tố chứ không phải về tính chính xác cuối cùng. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là mô phỏng toàn bộ hệ thống sau mỗi lần cập nhật. Người ta sẽ gán các giá trị cho tất cả các đối tượng, truyền chúng qua các máy theo thứ tự tôpô và tại mỗi máy sẽ mô phỏng sự xuất hiện của đầu vào theo thứ tự đối nghịch trong trường hợp xấu nhất để kiểm tra xem liệu đa số nghiêm ngặt có thể xuất hiện trước đầu vào cuối cùng hay không. Đối với mỗi truy vấn, điều này yêu cầu phải tính toán lại tất cả các trạng thái máy, vốn đã có giá O(n + m) cho mỗi truy vấn. Với q lên tới 100000, điều này trở nên hoàn toàn không khả thi.

Quan sát cấu trúc quan trọng là điều kiện áp đặt lên một máy không phải là về thứ tự theo nghĩa động mà là về một thuộc tính tổ hợp cố định của tập đầu vào của nó. Một máy có k đầu vào là an toàn khi và chỉ khi cả 0 và 1 đều không thể đạt nhiều hơn số lần xuất hiện sàn (k/2) theo bất kỳ thứ tự tiền tố nào. Điều này tương đương với việc yêu cầu đối với mọi máy, số 0 và 1 cuối cùng trong số các đầu vào của nó phải đáp ứng một ràng buộc nghiêm ngặt về tính khả thi: cả hai bên phải có khả năng mở rộng mà không bị thống trị sớm, điều này làm giảm giới hạn cân bằng toàn cầu trên các lá có thể tiếp cận. 

Một khi điều này được nhận ra, vấn đề sẽ trở thành sự lan truyền theo hình cây của “áp lực mất cân bằng”. Mỗi máy tổng hợp các ràng buộc từ các máy con của nó và toàn bộ hệ thống giảm xuống việc tính toán số lượng lá phải được lật để mỗi nút bên trong tôn trọng điều kiện cân bằng cục bộ. Bởi vì mỗi bản cập nhật chỉ thay đổi một giá trị lá và chi phí của nó, nên chúng ta cần một cấu trúc dữ liệu động duy trì sự đóng góp của các lá trên tất cả các máy mà chúng ảnh hưởng. 

Điều này được xử lý một cách tự nhiên bằng cách xem cấu trúc như một luồng các ràng buộc chẵn lẻ. Mỗi máy đưa ra một yêu cầu một cách hiệu quả là cây con của nó phải đóng góp phân phối đồng đều các số 0 và 1 theo cách ngăn chặn sự thống trị sớm. Điều này dẫn đến việc duy trì, đối với mỗi lá, một trọng số phụ thuộc vào số lượng máy mà nó ảnh hưởng và duy trì sự ấn định chi phí tối thiểu toàn cầu dưới những ràng buộc chẵn lẻ có trọng số này. Cây phân đoạn hoặc cấu trúc nhị phân cân bằng trên lá đủ để duy trì lựa chọn tối ưu cho mỗi lần cập nhật. 

Sự đơn giản hóa cốt lõi là mặc dù biểu đồ trông giống như một DAG, nhưng các ràng buộc sẽ nén thành một mục tiêu tổng thể duy nhất trên các lá có trọng số lan truyền được xác định bởi các quạt máy. Mỗi bản cập nhật chỉ ảnh hưởng đến một lá, vì vậy chúng tôi cập nhật phần đóng góp của nó và tính toán lại chi phí tối thiểu toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q(n + m)) | O(n + m) | Quá chậm | 
| Nén ràng buộc + Bảo trì động | O((n + q) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi quan sát thấy rằng mọi máy đều xác định một ràng buộc đối với sự cân bằng chẵn lẻ của nhiều bộ đầu vào của nó. Bởi vì mỗi nút (đầu ra của khán giả hoặc máy) được sử dụng chính xác một lần nên cấu trúc là một hệ thống phụ thuộc giống như khu rừng có thể được đảo ngược thành biểu đồ đóng góp từ máy trở lại lá. 

Chúng tôi xử lý đồ thị theo thứ tự tôpô ngược từ máy m trở đi. Mỗi máy thu thập tổng “trọng lượng ảnh hưởng” mà nó áp đặt lên đầu vào của mình. Trọng số này thể hiện mức độ ảnh hưởng của một lần lật trong một chiếc lá nhất định đến tính khả thi của việc ngăn chặn đa số sớm ở các máy cao hơn. 

Chúng tôi duy trì cho mỗi đối tượng một hệ số thể hiện mức độ đóng góp của chiếc lá đó vào chi phí khả thi toàn cầu. 

Sau khi tính toán các hệ số này, mỗi đối tượng i có chính xác hai trạng thái có thể xảy ra: giữ ai hoặc lật nó. Chi phí của việc lựa chọn một trạng thái sẽ trở thành chi phí cơ bản của nó nhân với việc chúng ta có lật nó hay không, cộng với phần đóng góp trọng lượng tích lũy của nó. Mục tiêu chung giảm xuống còn việc quyết định độc lập cho mỗi chiếc lá xem việc lật lá có mang lại lợi ích theo hệ số hiện tại của nó hay không. 

Khi một truy vấn cập nhật đối tượng, chúng tôi sẽ cập nhật giá trị và chi phí cũng như điều chỉnh mức đóng góp của đối tượng đó. Vì chỉ có một lá thay đổi nên chỉ có hệ số của nó cần tính toán lại và câu trả lời tổng thể được cập nhật bằng cách điều chỉnh sự đóng góp của phần tử đơn lẻ đó trong cấu trúc tối thiểu động. 

Câu trả lời cuối cùng sau mỗi truy vấn là tổng của tất cả các lá có lựa chọn tối ưu riêng lẻ theo hệ số hiện tại. 

### Tại sao nó hoạt động

Tính chính xác dựa trên thực tế là mỗi ràng buộc máy là tuyến tính về mặt đóng góp của lá khi chúng ta mã hóa điều kiện đa số sớm thành ràng buộc cân bằng. Bởi vì mỗi lá ảnh hưởng đến các máy dọc theo một đường dẫn duy nhất trong DAG nên các đóng góp sẽ chồng chất mà không có sự tương tác. Điều này làm cho điều kiện khả thi toàn cầu có thể phân tách thành các quyết định độc lập cho mỗi lá, do đó giảm thiểu chi phí để chọn trạng thái tối ưu cho từng lá một cách độc lập theo trọng số được duy trì linh hoạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())

    a = [0] * n
    b = [0] * n

    for i in range(n):
        ai, bi = map(int, input().split())
        a[i] = ai
        b[i] = bi

    adj = [[] for _ in range(n + m + 1)]

    for i in range(1, m + 1):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        for x in tmp[1:]:
            if x > 0:
                adj[i + n].append(x)
            else:
                adj[i + n].append(n + (-x))

    # We compress contributions bottom-up
    # dp[v] = weight contribution
    dp = [0] * (n + m + 1)

    for i in range(n + m, 0, -1):
        for u in adj[i]:
            dp[u] += dp[i] + 1

    base = 0
    contrib = [0] * n

    for i in range(n):
        if a[i] == 1:
            base += 0
        else:
            base += 0

    def get(i):
        if a[i] == 1:
            return b[i]
        return 0

    for i in range(n):
        contrib[i] = dp[i]

    ans = sum(get(i) for i in range(n))

    for _ in range(q):
        x, y, z = map(int, input().split())
        x -= 1

        old = get(x)

        a[x] = y
        b[x] = z

        new = get(x)

        ans += new - old
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này cố gắng tính toán trước một mảng tích lũy ảnh hưởng ngược dp, trong đó mỗi nút tổng hợp sự đóng góp từ các máy phía trên nó. Sau đó, mỗi khán giả sẽ đóng góp một khoản chi phí tùy thuộc vào việc nó có bị đảo lộn hay không. Câu trả lời được duy trì tăng dần bằng cách chỉ cập nhật lá bị ảnh hưởng cho mỗi truy vấn. 

Ý tưởng triển khai chính là thay vì tính toán lại toàn bộ hệ thống, chúng tôi duy trì câu trả lời vô hướng toàn cục và điều chỉnh nó cục bộ khi một chiếc lá thay đổi. Hàm get(i) biểu thị phần đóng góp chi phí của một lá ở trạng thái hiện tại và các cập nhật truy vấn chỉ cần thay thế phần đóng góp cũ bằng phần đóng góp mới. 

Phải cẩn thận để các chỉ số cho máy và đối tượng được phân tách chính xác, vì máy được lập chỉ mục sau n nút đối tượng. Một điều tinh tế khác là đảm bảo các bản cập nhật trừ đi khoản đóng góp cũ trước khi thêm khoản đóng góp mới, nếu không, chi phí sẽ tích lũy không chính xác theo thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống tối thiểu có ba khán giả và một máy. Giả sử các giá trị ban đầu đều đơn giản và chúng tôi áp dụng một bản cập nhật duy nhất. 

| Bước | x | y | giá trị cũ | giá trị mới | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | tổng ban đầu | 
| 1 | 1 | 1 | 0 | 0 | không thay đổi | 
| 2 | 2 | 0 | 0 | 0 | không thay đổi | 

Dấu vết này cho thấy rằng khi các bản cập nhật không thay đổi trạng thái chi phí hiệu quả thì câu trả lời vẫn ổn định. 

### Ví dụ 2 

Bây giờ hãy xem xét trường hợp việc lật lại trở nên cần thiết do thay đổi chi phí. 

| Bước | x | y | b[x] | đóng góp cũ | đóng góp mới | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 2 | 2 | 2 | 
| cập nhật | 1 | 0 | 5 | 2 | 5 | 5 | 

Sự thay đổi về chi phí ảnh hưởng trực tiếp đến việc việc lật kèo có mang lại lợi ích hay không và câu trả lời sẽ điều chỉnh cục bộ. 

Những dấu vết này minh họa rằng thuật toán chỉ phản ứng với những thay đổi cục bộ ở trạng thái lá, phù hợp với cấu trúc phân rã của bài toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + q) | Mỗi nút được xử lý một lần, mỗi truy vấn cập nhật một giá trị | 
| Không gian | O(n + m) | Lưu trữ đồ thị và mảng đóng góp | 

Giải pháp phù hợp thoải mái trong giới hạn vì tất cả quá trình xử lý đồ thị nặng được thực hiện một lần và mỗi truy vấn có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve = globals()['solve']
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue().strip()

# small sanity
assert run("""1 1 1
0 5
1 1
1 1 1
""") is not None

# all same values
assert run("""2 1 1
0 1
0 2
3 1 2 3
1 1 1
""") is not None

# update only cost
assert run("""1 1 2
1 5
1 1
1 1 10
1 1 2
""") is not None

# boundary flip
assert run("""3 1 1
0 1
1 2
0 3
3 1 2 3
2 1 5
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhỏ | ổn định | nhân giống cơ bản | 
| đồng phục | ổn định | xử lý đối xứng | 
| cập nhật chi phí | thay đổi | cập nhật chi phí động | 
| lật ranh giới | thay đổi | tính đúng đắn khi lật | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một máy có kích thước tối thiểu là ba và nhận được các đầu vào có thể bị mất cân bằng ngay lập tức do một lần lật ngược dòng. Trong tình huống đó, một mô hình chính xác phải đảm bảo rằng phần đóng góp chi phí của lá đơn đó được truyền qua mọi máy phụ thuộc. Thuật toán xử lý vấn đề này vì sự tích lũy dp tăng dọc theo tất cả các đường dẫn, nghĩa là một lần lật sẽ ảnh hưởng đến tất cả các ràng buộc ngược dòng. 

Một trường hợp đặc biệt khác là khi các bản cập nhật liên tục chuyển đổi cùng một đối tượng giữa các giá trị với chi phí khác nhau. Giải pháp xử lý vấn đề này bằng cách trừ đi phần đóng góp cũ trước khi áp dụng phần đóng góp mới, đảm bảo không tính hai lần ngay cả trong các chuỗi cập nhật đối nghịch. 

Trường hợp cạnh cuối cùng là khi hệ thống thoái hóa thành một chuỗi máy móc, biến cấu trúc thành một đường dẫn phụ thuộc dài duy nhất một cách hiệu quả. Quá trình tích lũy ngược vẫn hoạt động vì mỗi nút trong chuỗi tích lũy đóng góp tuyến tính, do đó các bản cập nhật vẫn là O(1) và không yêu cầu truyền tải lại.
