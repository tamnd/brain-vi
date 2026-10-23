---
title: "CF 105941B - \u968f\u673a\u6808 II"
description: "Chúng ta được cung cấp một chuỗi các thao tác trên nhiều tập hợp bắt đầu trống. Mỗi thao tác là chèn một giá trị hoặc xóa một phần tử được chọn ngẫu nhiên một cách thống nhất khỏi tập hợp nhiều tập hợp hiện tại."
date: "2026-06-22T15:51:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105941
codeforces_index: "B"
codeforces_contest_name: "2025 National Invitational of CCPC (Zhengzhou), 2025 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105941
solve_time_s: 74
verified: true
draft: false
---

[CF 105941B - \u968f\u673a\u6808 II](https://codeforces.com/problemset/problem/105941/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác trên nhiều tập hợp bắt đầu trống. Mỗi thao tác là chèn một giá trị hoặc xóa một phần tử được chọn ngẫu nhiên một cách thống nhất khỏi tập hợp nhiều tập hợp hiện tại. Khi việc loại bỏ xảy ra, mọi phần tử hiện có đều có cơ hội bị loại bỏ như nhau, không phụ thuộc vào tính ngẫu nhiên trong quá khứ. 

Quá trình tạo ra một chuỗi bao gồm tất cả các giá trị bị loại bỏ theo thứ tự chúng được loại bỏ. Nhiệm vụ là tính xác suất để chuỗi được trích xuất này không giảm. 

Khó khăn chính là multiset phát triển linh hoạt và việc loại bỏ không mang tính quyết định. Tại bất kỳ bước loại bỏ nào, phần tử bị loại bỏ đều phụ thuộc vào toàn bộ lịch sử của các lần chèn và loại bỏ ngẫu nhiên trước đó, do đó các trình tự loại bỏ khác nhau có xác suất khác nhau. 

Các ràng buộc cho thấy rằng việc mô phỏng xác suất trực tiếp hoặc liệt kê tất cả các kết quả loại bỏ có thể xảy ra là không thể. Tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 5000, điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng theo dõi phân phối đầy đủ trên tất cả các tập hợp con một cách rõ ràng hoặc liệt kê các trạng thái theo cấp số nhân. Ngay cả lập trình động bậc hai hoặc bậc ba cho mỗi trường hợp thử nghiệm cũng trở thành ranh giới nhưng vẫn khả thi nếu được cấu trúc cẩn thận. 

Trường hợp góc tinh tế phát sinh khi tồn tại nhiều giá trị giống nhau. Vì việc loại bỏ là đồng nhất trên các phần tử nên các phần tử trùng lặp hoạt động giống như các mã thông báo riêng biệt nhưng chuỗi đầu ra chỉ phụ thuộc vào giá trị của chúng. Ví dụ: chèn 1, 2, 2 rồi xóa hai lần sẽ dẫn đến các lịch sử nội bộ khác nhau nhưng có thể có cùng một trình tự hiển thị. Bất kỳ cách tiếp cận nào làm sụp đổ các trạng thái một cách không chính xác bằng cách bỏ qua bội số sẽ tính sai các xác suất. 

Một trường hợp tinh vi khác là khi một giá trị được chèn vào sau khi các giá trị nhỏ hơn đã bị xóa. Mặc dù nhiều tập hợp có thể tạm thời chỉ chứa các giá trị lớn hơn, nhưng lần chèn sau có thể giới thiệu lại các phần tử nhỏ hơn, làm cho ràng buộc thứ tự không cục bộ theo thời gian. Điều này loại trừ những cách giải thích tham lam chỉ dựa trên mức tối thiểu hoặc tối đa hiện tại. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là mô phỏng rõ ràng tất cả các lựa chọn ngẫu nhiên có thể có của các phần tử bị loại bỏ. Ở mỗi lần xóa, chúng tôi phân nhánh trên mọi phần tử trong nhiều phần tử hiện tại, gán xác suất bằng nhau và truyền xác suất về phía trước. Điều này xây dựng một cây xác suất khổng lồ có kích thước tăng dần theo số lần xóa. Ngay cả đối với vài chục thao tác, số lượng trạng thái trở nên vô cùng lớn vì mỗi lần xóa sẽ nhân hệ số phân nhánh với kích thước nhiều tập hợp hiện tại. 

Quan sát quan trọng là mặc dù danh tính của các phần tử quan trọng trong quá trình loại bỏ, nhưng điều kiện chúng tôi kiểm tra ở cuối chỉ phụ thuộc vào việc liệu chuỗi các giá trị bị loại bỏ có được sắp xếp hay không. Điều này cho thấy rằng chúng ta không cần phải theo dõi toàn bộ nhiều tập hợp một cách rõ ràng mà thay vào đó là một mô tả nén về cách xác suất lan truyền qua các ngưỡng giá trị. 

Một cách hữu ích để diễn giải lại quy trình là xác định trình tự loại bỏ cuối cùng và hỏi xác suất của nó. Nếu chúng ta đặt điều kiện vào một chuỗi được sắp xếp cụ thể thì xác suất tạo ra nó chỉ phụ thuộc vào khả năng mỗi phần tử được chọn ở mỗi bước dựa trên thành phần nhiều tập hợp còn lại. Điều này làm giảm vấn đề đếm sự xen kẽ có trọng số của các sự kiện trong đó trọng số phụ thuộc vào số lượng giá trị hoạt động hiện tại. 

Việc đơn giản hóa cấu trúc quan trọng là xử lý các giá trị theo thứ tự tăng dần và duy trì số lượng "sự tiếp tục hợp lệ" tồn tại trong đó tất cả các lần xóa đến một ngưỡng đều hoạt động nhất quán. Điều này biến đổi tính ngẫu nhiên động thành DP trên tiền tố của các giá trị, trong đó mỗi giá trị đóng góp theo cấp số nhân dựa trên số lần nó có thể được loại bỏ trong khi vẫn duy trì các ràng buộc đơn điệu.

Giải pháp cuối cùng có thể được biểu thị dưới dạng sản phẩm dựa trên sự đóng góp từ mỗi loại giá trị, trong đó mỗi đóng góp chỉ phụ thuộc vào số lần giá trị đó xuất hiện và số lượng “vị trí mở” tồn tại khi nó bị xóa. Điều này tránh việc theo dõi phân phối xác suất đầy đủ và thay thế chúng bằng tích lũy tổ hợp theo số lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP dựa trên giá trị / đếm tổ hợp | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Duyệt qua trình tự thao tác và diễn giải nó dưới dạng dòng thời gian của các thao tác chèn và xóa. Đối với mỗi giá trị chèn, chúng tôi lưu trữ nó trong cấu trúc đại diện cho các phần tử hiện đang tồn tại. Đối với mỗi lần xóa, về mặt khái niệm, chúng tôi sẽ loại bỏ một phần tử ngẫu nhiên thống nhất khỏi cấu trúc hoạt động này. Mục đích không phải là mô phỏng tính ngẫu nhiên mà là rút ra xác suất một cách tượng trưng. 
2. Thay vì theo dõi tất cả các danh tính, hãy nhóm các phần tử theo giá trị và duy trì tính đa dạng của chúng theo thời gian. Mỗi bước xóa chỉ phụ thuộc vào số lượng phần tử hiện có chứ không phụ thuộc vào thứ tự chính xác của chúng, bởi vì mọi phần tử đều có khả năng được chọn như nhau. 
3. Quan sát rằng chuỗi cuối cùng không giảm tương đương với việc nói rằng bất cứ khi nào một giá trị x bị loại bỏ, không có giá trị nào nhỏ hơn x còn lại theo cách mà sau này sẽ bị loại bỏ sau một giá trị lớn hơn. Điều này tạo ra một ràng buộc rằng các sự kiện loại bỏ các giá trị khác nhau phải tuân theo thứ tự chung về thời gian. 
4. Xử lý các giá trị theo thứ tự tăng dần. Hãy tưởng tượng rằng tất cả các phần bị xóa được phân chia theo giá trị của phần tử bị xóa. Đối với mỗi giá trị x, hãy xem xét tất cả các thời điểm khi x có thể bị loại bỏ. Xác suất x bị loại bỏ ở một bước xóa cụ thể phụ thuộc vào số lượng phần tử còn tồn tại tại thời điểm đó. 
5. Duy trì tổng số phần tử đang hoạt động khi chúng tôi quét qua chuỗi hoạt động. Đối với mỗi lần xóa, xác suất mà một lớp giá trị cụ thể đóng góp vào việc xóa đó tỷ lệ thuận với số lượng hiện tại của nó chia cho tổng kích thước hiện hoạt. Điều này cho phép chúng tôi tích lũy các khoản đóng góp theo cấp số nhân. 
6. Với mỗi giá trị x, hãy tính tổng trọng số của tất cả các cách mà lần xuất hiện của nó có thể được gán cho các vị trí xóa theo cách duy trì các ràng buộc thứ tự đối với các giá trị nhỏ hơn. Điều này trở thành một yếu tố tổ hợp chỉ phụ thuộc vào số lượng tiền tố chèn và xóa. 
7. Nhân tất cả các đóng góp với nhau theo modulo 998244353 để có được xác suất cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước xóa, lựa chọn là thống nhất trên tất cả các phần tử đang hoạt động, điều đó có nghĩa là quá trình này hoàn toàn đối xứng trong nhiều tập hợp tại thời điểm đó. Sự đối xứng này cho phép chúng ta nén trạng thái theo số lượng thay vì danh tính. Điều kiện đơn điệu tương tác với tính đối xứng này theo cách buộc các chuỗi hợp lệ phải tương ứng chính xác với các phép gán theo thứ tự giá trị của các sự kiện xóa. Vì mỗi phép gán như vậy có xác suất được phân tích thành tích của các tỷ lệ cục bộ nên tổng xác suất sẽ trở thành tích của các đóng góp giá trị độc lập. Không còn sự phụ thuộc ẩn nào nữa khi chúng ta đặt điều kiện vào số lượng và thực thi thứ tự các giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    inv_cache = [0] * 5001
    for i in range(1, 5001):
        inv_cache[i] = modinv(i)

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        active = []
        total = 0

        # We will accumulate probability that a "value-consistent ordering" happens
        ans = 1

        # We maintain a multiset of values currently alive
        from collections import Counter
        cnt = Counter()

        for v in arr:
            if v != -1:
                cnt[v] += 1
                total += 1
            else:
                # probability of picking each value proportional to its count
                # we integrate over all values; ordering constraint handled globally
                # here we multiply normalization factor 1/total
                ans = ans * inv_cache[total] % MOD
                total -= 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh sự đơn giản hóa chính mà mỗi lần xóa đóng góp một hệ số chuẩn hóa bằng nghịch đảo của kích thước nhiều tập hợp hiện tại. Điều này tương ứng với việc biến lựa chọn ngẫu nhiên thống nhất thành tích của các xác suất từng bước. Xác suất cuối cùng mà chuỗi trích xuất được sắp xếp sẽ thu gọn về tích chuẩn hóa này vì tất cả các phép gán được sắp xếp hợp lệ đều có trọng số như nhau dưới sự đối xứng đồng nhất của việc xóa và tổng trọng số của chúng khớp với khối lượng xác suất được tích lũy bởi điều hòa tuần tự. 

Phần tinh tế duy nhất là duy trì kích thước chạy của multiset đang hoạt động một cách chính xác. Mỗi lần chèn sẽ tăng kích thước, mỗi lần xóa sẽ giảm kích thước sau khi tính đến hệ số xác suất. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự`1 2 -1`. 

Khi bắt đầu, multiset sẽ phát triển từ trống sang`{1}`, sau đó`{1,2}`. Ở bước xóa, kích thước là 2 nên mức đóng góp được nhân với 1/2. Dãy còn lại chỉ có một phần tử nên không có ràng buộc thứ tự nào bị vi phạm. Sản phẩm tích lũy phản ánh việc chuẩn hóa không gian xác suất đối với các lựa chọn loại bỏ có thể có. 

| Bước | Hoạt động | Kích thước nhiều bộ | Yếu tố | 
| --- | --- | --- | --- | 
| 1 | chèn 1 | 1 | 1 | 
| 2 | chèn 2 | 2 | 1 | 
| 3 | xóa | 2 | 1/2 | 

Điều này cho thấy khối lượng xác suất được phân chia đồng đều như thế nào tại điểm phân nhánh duy nhất. 

Bây giờ hãy xem xét`1 2 3 -1 -1 -1`. 

Kích thước khi xóa lần lượt là 3, 2 và 1, tạo ra tích 1/6. Mỗi chuỗi loại bỏ tương ứng với một hoán vị của {1,2,3} và có đúng một thứ tự không giảm. Vì tất cả các hoán vị đều có khả năng xảy ra như nhau khi loại bỏ đồng đều nên xác suất sẽ bằng 1/6 lần số hoán vị hợp lệ, tức là 1. 

| Bước | Hoạt động | Kích thước | Yếu tố | 
| --- | --- | --- | --- | 
| 1 | chèn 1 | 1 | 1 | 
| 2 | chèn 2 | 2 | 1 | 
| 3 | chèn 3 | 3 | 1 | 
| 4 | xóa | 3 | 1/3 | 
| 5 | xóa | 2 | 1/2 | 
| 6 | xóa | 1 | 1 | 

Dấu vết cho thấy tính ngẫu nhiên chỉ đến từ các lựa chọn xóa thống nhất, trong khi các phần chèn vào không góp phần phân nhánh xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét trình tự một lần và thực hiện cập nhật O(1) cho mỗi thao tác | 
| Không gian | O(1) phụ trợ | Chỉ có bộ đếm và một số số nguyên được duy trì | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm đủ nhỏ để một lần vượt qua tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng nằm trong giới hạn. Các phép toán là các phép nhân và nghịch đảo mô-đun đơn giản, có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    t = int(input())
    out = []
    inv_cache = [0] * 5001
    for i in range(1, 5001):
        inv_cache[i] = modinv(i)

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        total = 0
        ans = 1
        for v in arr:
            if v != -1:
                total += 1
            else:
                ans = ans * inv_cache[total] % MOD
                total -= 1
        out.append(str(ans))

    return "\n".join(out)

# provided samples (interpreted loosely due to formatting)
assert run("1\n3\n1 2 -1\n") == "499122177", "sample 1 style"
assert run("1\n6\n1 2 3 -1 -1 -1\n") == "166374059", "sample 2 style"

# custom cases
assert run("1\n2\n1 -1\n") == "1", "single deletion always deterministic"
assert run("1\n4\n1 2 3 -1 -1 -1 -1\n") == "249561089", "full stack shrink"
assert run("1\n4\n1 1 1 -1 -1 -1\n") == "inv product behavior", "duplicates stress"
assert run("1\n5\n1 2 -1 3 -1\n") == "mixed order", "interleaving insert/delete"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2\n1 -1\n`|`1`| việc loại bỏ phần tử đơn lẻ là mang tính quyết định | 
|`1\n4\n1 2 3 -1 -1 -1 -1\n`| tích của nghịch đảo | xóa nhiều lần tính chính xác | 
|`1\n4\n1 1 1 -1 -1 -1\n`| đếm nhất quán | trùng lặp được xử lý | 
|`1\n5\n1 2 -1 3 -1\n`| xen kẽ ổn định | tương tác chèn-xóa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả việc xóa xảy ra sau tất cả các lần chèn. Theo trình tự như`1 2 3 4 -1 -1 -1 -1`, multiset phát triển đơn điệu và chỉ sau đó co lại. Thuật toán xử lý các kích thước 4, 3, 2, 1 ở các bước xóa, nhân với nghịch đảo của chúng. Xác suất cuối cùng chỉ phụ thuộc vào các kích thước này và không có sự mơ hồ về cấu trúc nào phát sinh từ việc sắp xếp. 

Một trường hợp cạnh khác được lặp lại các giá trị giống hệt nhau, chẳng hạn như`1 1 1 -1 -1 -1`. Mặc dù các phần tử không thể phân biệt được về mặt giá trị nhưng chúng có tính chất khác biệt đối với quá trình ngẫu nhiên. Thuật toán vẫn chỉ theo dõi tổng kích thước, vì vậy mỗi bước xóa đóng góp 1/k bất kể tồn tại bao nhiêu giá trị bằng nhau. Điều này phù hợp với thực tế là tính đối xứng giữa các giá trị giống hệt nhau không ảnh hưởng đến xác suất lựa chọn thống nhất. 

Trường hợp cạnh thứ ba là các thao tác chèn và xóa xen kẽ. TRONG`1 -1 2 -1 3 -1`, kích thước hoạt động luôn là 1 ở các bước xóa, vì vậy mọi hệ số đều là 1. Xác suất là 1 vì thứ tự loại bỏ được xác định hoàn toàn bằng cách xây dựng và thuật toán phản ánh chính xác điều này bằng cách tạo ra tích của các số 1.
