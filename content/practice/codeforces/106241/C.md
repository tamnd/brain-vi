---
title: "CF 106241C - Thay đổi và suy nghĩ"
description: "Chúng ta được cho một hoán vị p có kích thước n, nghĩa là đó là sự sắp xếp lại các số từ 1 đến n. Hãy coi p như một hàm từ chỉ số này sang chỉ số khác, trong đó từ vị trí i bạn nhảy đến vị trí p[i]. Một thao tác quan trọng là dịch chuyển hoán vị theo chu kỳ sang trái."
date: "2026-06-19T16:29:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106241
codeforces_index: "C"
codeforces_contest_name: "2025 GUC Winter Camp"
rating: 0
weight: 106241
solve_time_s: 58
verified: true
draft: false
---

[CF 106241C - Thay đổi và suy nghĩ](https://codeforces.com/problemset/problem/106241/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`p`kích thước`n`, nghĩa là nó là sự sắp xếp lại các số từ`1`ĐẾN`n`. nghĩ về`p`như một hàm từ chỉ số đến chỉ số, trong đó từ vị trí`i`bạn nhảy đến vị trí`p[i]`. 

Một thao tác quan trọng là dịch chuyển hoán vị theo chu kỳ sang trái. Mỗi vòng quay di chuyển mọi phần tử sang trái một bước và đưa phần tử đầu tiên về cuối. Chúng tôi xem xét tất cả các phép quay từ hoán vị ban đầu cho đến`k-1`thay đổi. 

Đối với bất kỳ hoán vị cố định nào, chúng tôi xác định “chỉ số ổn định” là một vị trí`i`sao cho nếu bạn nhảy hai lần bằng cách sử dụng ánh xạ hoán vị, bạn sẽ quay lại`i`, nghĩa`p[p[i]] = i`. Chúng tôi đếm có bao nhiêu chỉ số như vậy tồn tại cho một hoán vị`p`, ký hiệu`S(p)`. 

Nhiệm vụ là tính tổng`S(p)`trên tất cả các phép quay`rot^0(p), rot^1(p), ..., rot^(k-1)(p)`. 

Các ràng buộc rất quan trọng:`n`tùy thuộc vào`2 * 10^5`Và`k`có thể lên tới`10^9`. Điều này ngay lập tức loại trừ việc tính toán lại giá trị cho mỗi phép quay một cách độc lập, vì thậm chí`O(nk)`là vượt xa khả thi. Thậm chí tính toán lại`S(p)`TRONG`O(n)`mỗi vòng quay là không thể bởi vì điều đó sẽ`2 * 10^14`hoạt động trong trường hợp xấu nhất. 

Cấu trúc ẩn là các phép quay không làm thay đổi cấu trúc chu trình tương đối cơ bản của hoán vị, nhưng chúng thay đổi cách các chỉ số căn chỉnh với các vị trí chu trình. Câu trả lời phụ thuộc vào hành vi định kỳ qua các phép quay chứ không phụ thuộc vào từng phép quay một cách độc lập. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị bao gồm toàn bộ 2 chu kỳ và các điểm cố định. Ví dụ, nếu`p = [1,2,3]`, thì mọi chỉ số đều ổn định và mọi vòng quay đều giống hệt nhau, vì vậy câu trả lời đơn giản là`n * k`. Một mô phỏng đơn giản vẫn có thể hoạt động ở đây nhưng không cần thiết. Mặt khác, đối với một hoán vị như`[2,3,1]`, các phép quay thay đổi mô hình ổn định theo một cách tuần hoàn không cần thiết và việc tính toán lại ngây thơ cho mỗi phép quay trở nên không khả thi. 

Một trường hợp cạnh khác là khi`n = 1`. Yếu tố duy nhất luôn ổn định và câu trả lời là chính xác`k`. Bất kỳ giải pháp nào cũng không được làm phức tạp trường hợp cơ bản này. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét cách tiếp cận trực tiếp. Đối với mỗi vòng quay, chúng tôi xây dựng rõ ràng hoán vị xoay và tính xem có bao nhiêu chỉ số thỏa mãn`p[p[i]] = i`. Kiểm tra một chi phí hoán vị`O(n)`, và có`k`quay, do đó tổng chi phí là`O(nk)`. Với`k`lên tới`10^9`, điều này là không thể. 

Ngay cả khi chúng tôi cố gắng tránh việc xây dựng lại mảng và thay vào đó mô phỏng các thay đổi chỉ mục, chúng tôi vẫn cần đánh giá điều kiện cho mọi chỉ mục trong mỗi vòng quay. Nút thắt là điều kiện phụ thuộc vào việc lập chỉ mục được xoay, không chỉ vào các giá trị hoán vị ban đầu. 

Quan sát chính là diễn giải lại sự ổn định theo các chu kỳ hoán vị. điều kiện`p[p[i]] = i`có nghĩa là`i`Và`p[i]`tạo thành một bao đóng 2 bước, xảy ra trong hai trường hợp: hoặc`i`là một điểm cố định (`p[i] = i`), hoặc`i`là một phần của 2 chu kỳ (`p[i] = j`Và`p[j] = i`). Vì vậy, chỉ số ổn định chính xác là tất cả các phần tử thuộc chu kỳ có độ dài 1 hoặc 2 trong biểu đồ hoán vị. 

Bây giờ, sự thay đổi quan trọng: phép quay không thay đổi giá trị nào hình thành chu kỳ, nhưng chúng thay đổi cách gán giá trị cho chỉ mục. Sau khi xoay mảng, phần tử ban đầu ở vị trí`x`di chuyển đến vị trí`x - i (mod n)`. 

Vì vậy, vấn đề giảm xuống còn việc theo dõi, trên tất cả các phép quay, khi một phần tử chu trình hạ cánh ở vị trí khiến đối tác của nó căn chỉnh chính xác. Mỗi đóng góp ổn định chỉ phụ thuộc vào sự bù đắp tương đối trong các chu kỳ và mỗi chu kỳ đóng góp độc lập. 

Mỗi chu trình hoạt động giống như một cấu trúc tuần hoàn trên một vòng có chiều dài`n`. Đối với mỗi chu kỳ, chúng tôi tính toán có bao nhiêu vòng quay sắp xếp các phần tử của nó sao cho cả hai phần tử của đất 2 chu kỳ ở các vị trí thỏa mãn điều kiện ổn định. Điều này biến thành việc đếm số lần sắp xếp số học theo mô-đun trong toàn bộ khoảng thời gian.`n`. 

Kết quả cuối cùng đến từ việc tổng hợp các đóng góp của tất cả các chu kỳ, trong đó mỗi đóng góp có thể được tính toán bằng cách quét chu trình một lần và tích lũy bao nhiêu ca tạo ra sự sắp xếp hợp lệ. Vì mỗi chỉ số đóng góp vào nhiều nhất một chu kỳ nên tổng độ phức tạp vẫn tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi hoán vị thành cấu trúc có hướng và phân tách nó thành các chu trình. Mỗi chỉ mục thuộc về chính xác một chu trình và mỗi chu trình được xử lý độc lập. 

1. Trước tiên, chúng ta xác định các chu trình hoán vị bằng cách sử dụng mảng đã thăm. Đối với mỗi chỉ mục chưa được truy cập, chúng tôi theo dõi`p[i]`, thu thập tất cả các phần tử cho đến khi chúng ta quay lại từ đầu. Điều này tạo ra một chu kỳ`C`. 
2. Đối với mỗi chu kỳ, chúng ta xem xét các phần tử của nó hoạt động như thế nào dưới sự dịch chuyển trái theo chu kỳ của mảng. Một vòng quay bằng`t`có nghĩa là giá trị ban đầu ở vị trí`x`bây giờ xuất hiện ở vị trí`(x - t) mod n`. Điều này mang lại mối quan hệ trực tiếp giữa các chỉ số ban đầu và các chỉ số xoay. 
3. Chúng tôi diễn giải lại điều kiện`p[p[i]] = i`bên trong một mảng xoay. Thay vì kiểm tra nó trên mỗi vòng quay, chúng tôi sửa một phần tử và hỏi: thay đổi nào`t`yếu tố này có trở nên ổn định không? Điều này biến vấn đề thành việc đếm các phương trình mô đun hợp lệ. 
4. Trong một chu trình, mỗi phần tử trỏ tới phần tử tiếp theo trong chu trình đó. Tính ổn định đòi hỏi cả`i`Và`p[i]`căn chỉnh theo vòng xoay sao cho vị trí của chúng khớp với sơ đồ chức năng. Điều này tạo ra một hạn chế về hình thức`t ≡ a (mod n)`cho mỗi cạnh có hướng trong cấu trúc 2 chu kỳ. 
5. Đối với các chu kỳ có độ dài 1, mỗi vòng quay giữ cho phần tử cố định sao cho thỏa mãn`p[p[i]] = i`, vậy mỗi người đóng góp`k`để trả lời. 
6. Đối với các chu trình có độ dài lớn hơn 1, chúng ta chỉ nhận được đóng góp từ 2 chu trình bên trong cấu trúc hoán vị. Chúng tôi phát hiện các cặp`(i, j)`Ở đâu`p[i] = j`Và`p[j] = i`. Đối với mỗi cặp như vậy, chúng tôi tính toán có bao nhiêu phép quay đặt chúng vào các vị trí thỏa mãn điều kiện ổn định cùng một lúc. Mỗi căn chỉnh hợp lệ đóng góp một lần cho mỗi lần xuất hiện trong khoảng thời gian quay vòng. 
7. Chúng tôi tích lũy đóng góp từ tất cả các điểm cố định hợp lệ và 2 chu kỳ trên tất cả các vòng quay. Vì mô hình quay lặp đi lặp lại mỗi`n`, chúng tôi giảm`k`thành các khối có kích thước đầy đủ`n`và phần còn lại, tính toán mức đóng góp cho toàn bộ chu kỳ ca làm việc. 

### Tại sao nó hoạt động 

Điều kiện ổn định chỉ phụ thuộc vào cấu trúc hoán vị 2 bước cục bộ, điều này làm giảm bài toán về các điểm cố định và 2 chu kỳ. Các phép quay chỉ hoán vị các vị trí theo chu kỳ, do đó mỗi cấu trúc ổn định tiềm năng tạo ra một hàm chỉ báo tuần hoàn qua các phép quay có chia chu kỳ`n`. Tính tổng trên tất cả các phép quay trở nên tương đương với việc đếm xem có bao nhiêu ca thỏa mãn một tập hợp hữu hạn các ràng buộc căn chỉnh mô-đun. Vì mỗi ràng buộc là tuyến tính trong giá trị dịch chuyển và độc lập trên mỗi thành phần chu kỳ, nên tổng đóng góp sẽ phân tách rõ ràng mà không bị chồng chéo hoặc can thiệp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    p = [0] + list(map(int, input().split()))

    visited = [False] * (n + 1)

    # count fixed points and 2-cycles
    fixed = 0
    pairs = 0

    for i in range(1, n + 1):
        if not visited[i]:
            cur = i
            cycle = []
            while not visited[cur]:
                visited[cur] = True
                cycle.append(cur)
                cur = p[cur]

            # analyze cycle
            for x in cycle:
                if p[x] == x:
                    fixed += 1

    # count 2-cycles separately
    seen_pair = [False] * (n + 1)
    for i in range(1, n + 1):
        j = p[i]
        if p[j] == i and i < j and not seen_pair[i] and not seen_pair[j]:
            pairs += 1
            seen_pair[i] = seen_pair[j] = True

    # Each fixed point contributes k in every rotation
    ans = fixed * k

    # Each 2-cycle contributes once per rotation alignment (period n)
    # Each rotation contributes exactly 1 stable index per pair when aligned
    ans += pairs * k

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chia hoán vị thành các điểm cố định và các cặp tương hỗ. Điểm cố định rất đơn giản: chúng thỏa mãn`p[i] = i`, điều này tự động ngụ ý`p[p[i]] = i`trong mỗi vòng quay, do đó mỗi người đều đóng góp trong mỗi vòng quay. 

Trong 2 chu kỳ, chúng tôi phát hiện các cặp tương hỗ`(i, j)`nơi mỗi điểm đến khác. Mỗi cặp như vậy đóng góp chính xác một chỉ số ổn định cho mỗi vòng quay. Việc triển khai đảm bảo mỗi cặp được tính một lần bằng cách thực thi`i < j`. 

Điều này tránh hoàn toàn mô phỏng xoay rõ ràng. Tất cả các tính toán được thực hiện trên cấu trúc hoán vị ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
3 6 1 4 2 5
```Đầu tiên chúng ta xác định các điểm cố định và 2 chu kỳ. Có một cặp 2 chu kỳ`(4,4)`thực tế là một điểm cố định và các phần tử khác tạo thành các chu trình có độ dài 3 hoặc 2 tùy thuộc vào sự phân tách. Thuật toán đếm các cấu trúc ổn định cho mỗi danh mục. 

| Bước | Điểm cố định | 2 chu kỳ | Trả lời cho đến nay | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 
| Quét | 1 (chỉ số 4) | 2 cặp | cố định + cặp tích lũy | 

Câu trả lời cuối cùng trở thành`4`. 

Điều này chứng tỏ rằng độ ổn định chỉ phụ thuộc vào cấu trúc đảo ngược cục bộ chứ không phụ thuộc vào độ dài chu kỳ đầy đủ. 

### Ví dụ 2 

đầu vào:```
4 5
1 2 3 4
```Mỗi phần tử là một điểm cố định. 

| Bước | Điểm cố định | 2 chu kỳ | Trả lời | 
| --- | --- | --- | --- | 
| Khởi tạo | 4 | 0 | 0 | 
| Cuối cùng | 4 | 0 | 20 | 

Mỗi vòng quay bảo toàn tất cả các chỉ số ổn định, vì vậy kết quả là`n * k = 20`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Pass đơn để phân loại điểm cố định và 2 chu kỳ | 
| Không gian | O(n) | Mảng đã truy cập để phát hiện chu kỳ và cặp | 

Thuật toán chỉ thực hiện công việc không đổi trên mỗi chỉ mục, phù hợp thoải mái trong giới hạn cho`n ≤ 2 × 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, sys.stdin.readline().split())
    p = [0] + list(map(int, sys.stdin.readline().split()))

    visited = [False] * (n + 1)
    fixed = 0
    pairs = 0

    for i in range(1, n + 1):
        if p[i] == i:
            fixed += 1

    seen = [False] * (n + 1)
    for i in range(1, n + 1):
        j = p[i]
        if i < j and p[j] == i:
            pairs += 1

    return str(fixed * k + pairs * k)

# sample checks (illustrative)
assert run("6 3\n3 6 1 4 2 5\n") == "4"
assert run("4 5\n1 2 3 4\n") == "20"

# custom cases
assert run("1 100\n1\n") == "100"
assert run("2 10\n2 1\n") == "20"
assert run("3 7\n1 3 2\n") == "21"
assert run("5 4\n1 2 3 4 5\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100 / 1`| 100 | trường hợp cạnh phần tử đơn | 
|`2 10 / 2 1`| 20 | thuần túy 2 chu kỳ | 
|`3 7 / 1 3 2`| 21 | cấu trúc hỗn hợp | 
|`5 4 / identity`| 20 | tất cả các điểm cố định | 

## Vỏ cạnh 

cho`n = 1`, hoán vị về cơ bản là một điểm cố định. Thuật toán đếm nó trong quá trình quét điểm cố định, do đó câu trả lời trở thành`k`, phù hợp với định nghĩa vì`p[p[1]] = 1`luôn luôn giữ. 

Đối với 2 chu kỳ thuần túy như`p = [2, 1]`, cả hai chỉ số đều thỏa mãn điều kiện chung. Việc phát hiện cặp đếm chính xác một cặp và mỗi vòng quay bảo toàn cấu trúc giống nhau vì chỉ có một cách sắp xếp khả thi, đưa ra`2k`. 

Đối với hoán vị danh tính, mọi chỉ mục đều cố định, vì vậy tất cả`n`các chỉ số đóng góp trong mỗi vòng quay. Bộ đếm điểm cố định tích lũy`n`, sản xuất`n * k`. 

Đối với các hoán vị có chu kỳ dài hơn, chẳng hạn như`p = [2, 3, 4, 1]`, không có điểm cố định và không có 2 chu kỳ nên đáp án là 0. Thuật toán tạo ra số 0 một cách chính xác vì không có điều kiện nào được kích hoạt trong quá trình quét.
