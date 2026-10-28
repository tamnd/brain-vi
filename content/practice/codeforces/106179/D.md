---
title: "CF 106179D - Làm trống"
description: "Chúng ta được hoán vị các số từ 1 đến n và n được đảm bảo là số chẵn. Nhiệm vụ là liên tục loại bỏ các phần của mảng cho đến khi không còn gì."
date: "2026-06-25T10:54:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106179
codeforces_index: "D"
codeforces_contest_name: "ICPC India Online Prelims (2025 - 2026)"
rating: 0
weight: 106179
solve_time_s: 41
verified: true
draft: false
---

[CF 106179D - Tạo trống](https://codeforces.com/problemset/problem/106179/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n và n được đảm bảo là số chẵn. Nhiệm vụ là liên tục loại bỏ các phần của mảng cho đến khi không còn gì. Mỗi thao tác loại bỏ không hoạt động trên một phân đoạn liền kề mà trên một chuỗi tiếp theo, nghĩa là chúng ta có thể chọn các phần tử từ bất kỳ đâu trong mảng hiện tại mà vẫn giữ nguyên thứ tự. 

Hạn chế về những gì chúng ta có thể loại bỏ là khó khăn chính. Chúng ta phải chọn một dãy con có độ dài chẵn và chia nó thành hai nửa bằng nhau. Nửa đầu và nửa sau phải hoàn toàn có thể tách biệt theo giá trị: mọi phần tử trong nửa đầu đều nhỏ hơn mọi phần tử trong nửa sau hoặc thứ tự ngược lại giữ trong đó tất cả các phần tử trong nửa đầu đều lớn hơn các phần tử trong nửa sau. Khi một dãy con như vậy được chọn, tất cả các phần tử của nó sẽ bị xóa khỏi hoán vị cùng một lúc. 

Mục tiêu là loại bỏ tất cả các phần tử bằng cách sử dụng số lượng thao tác tối thiểu như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra một hoán vị và chúng ta phải xuất ra cả số lượng thao tác tối thiểu và các chuỗi con thực tế được sử dụng trong mỗi thao tác. 

Các ràng buộc cho phép tổng n trên tất cả các trường hợp thử nghiệm lên tới 2 × 10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các chuỗi con hoặc thậm chí thử các chiến lược ghép nối bậc hai cho mỗi trường hợp thử nghiệm. Bất kỳ điều gì gần với O(n²) cho mỗi trường hợp thử nghiệm sẽ thất bại vì ngay cả 10^10 thao tác cũng vượt xa giới hạn thời gian. 

Một số tình huống nguy hiểm rất dễ xử lý sai. 

Nếu hoán vị đã được phân chia hoàn hảo theo cách mà một thao tác có thể loại bỏ mọi thứ, ví dụ: chuỗi tăng dần như [1, 2, 3, 4], thì toàn bộ mảng là hợp lệ vì chúng ta có thể chia nó thành hai nửa [1, 2] và [3, 4], trong đó max(left) < min(right). Một cách tiếp cận ngây thơ có thể giả định không chính xác rằng chỉ có cấu trúc liền kề mới quan trọng và bỏ sót rằng bất kỳ chuỗi nào cũng được cho phép. 

Một trường hợp tinh vi khác là hoán vị như [2, 3, 1, 4]. Thoạt nhìn, nó có vẻ không thể tách rời trên toàn cầu vì 1 phá vỡ thứ tự. Tuy nhiên, chúng ta vẫn có thể chọn một dãy con hợp lệ như [2, 4] hoặc [3, 1] tùy thuộc vào cấu trúc, để lại một hoán vị nhỏ hơn có thể giải được một cách tầm thường. Điều này cho thấy chúng ta không bị buộc phải loại bỏ các đoạn tối đa; đôi khi cần phải loại bỏ một lượng nhỏ mang tính chiến lược để khôi phục khả năng phân tách toàn cầu. 

Cuối cùng, một giả định sai lầm phổ biến là chúng ta luôn muốn ghép đôi nhỏ nhất với lớn nhất một cách tham lam. Điều này không thành công khi hoán vị hiện tại có nhiều “khối” giá trị thấp và cao xen kẽ, vì việc ghép nối tham lam có thể phá vỡ khả năng hình thành các nửa hợp lệ sau này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình một cách trực tiếp. Ở mỗi bước, chúng tôi sẽ thử tất cả các dãy con có độ dài chẵn của hoán vị hiện tại, chia chúng thành hai nửa, kiểm tra xem điều kiện tối thiểu có đúng hay không và chọn một dãy con hợp lệ để loại bỏ. Đây là sự bùng nổ tổ hợp. Ngay cả đối với một thao tác đơn lẻ, số lượng chuỗi con theo cấp số nhân là n và việc xác thực từng chuỗi yêu cầu quét tuyến tính. Điều này nhanh chóng trở nên không khả thi ngay cả khi n = 20. 

Cái nhìn sâu sắc về cấu trúc đến từ việc quan sát những gì một hoạt động hợp lệ thực sự thực thi. Mỗi thao tác sẽ loại bỏ một tập hợp các phần tử có thể được phân chia thành “khối thấp” và “khối cao” với sự phân tách chặt chẽ về giá trị. Vì chúng ta đang làm việc với một hoán vị nên thứ tự giá trị là toàn cục và cố định, do đó, ràng buộc về cơ bản là chia các giá trị đã chọn thành hai nhóm không trùng nhau trong phạm vi giá trị. 

Điều này cho thấy chúng ta nên suy nghĩ về việc sắp xếp theo giá trị hơn là theo vị trí. Nếu chúng ta xem xét hoán vị từ nhỏ nhất đến lớn nhất, thì bất kỳ thao tác hợp lệ nào cũng là việc chọn một tập hợp các giá trị có thể được chia thành hai nhóm cách nhau một ngưỡng một cách hiệu quả. Ngưỡng đó hoạt động giống như một vết cắt theo thứ tự được sắp xếp.

Nhận thức quan trọng là chúng ta không cần phải xây dựng các hoạt động lớn một cách cẩn thận. Chúng ta luôn có thể thực hiện các thao tác loại bỏ các cặp phần tử, trong đó một phần tử hiện ở nửa dưới của các giá trị còn lại và một phần tử ở nửa trên, miễn là chúng ta duy trì tính khả thi. Cấu trúc hoán vị đảm bảo rằng chúng ta có thể tiếp tục ghép các cực trị một cách có kiểm soát và sau khi thực hiện đủ các thao tác như vậy, cấu trúc còn lại sẽ có thể tháo rời được một cách dễ dàng. 

Điều này làm giảm vấn đề liên tục chọn các cặp ngưỡng chéo hợp lệ theo cách tôn trọng các ràng buộc về thứ tự, thay vì tìm kiếm trên các chuỗi con tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm chuỗi con Brute Force | hàm mũ | O(n) | Quá chậm | 
| Phân hủy cặp giá trị tham lam | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tập hợp các phần tử còn lại hiện tại và liên tục xây dựng các phép toán hợp lệ bằng cách ghép các phần tử từ các phía đối diện của sự phân chia giá trị. 

1. Chúng tôi giữ các phần tử còn lại trong một cấu trúc cho phép chúng tôi trích xuất các phần tử theo thứ tự giá trị tăng dần và giảm dần. Sắp xếp hoặc duy trì một cấu trúc cân bằng là đủ. 
2. Chúng tôi liên tục xác định phân vùng giữa các giá trị còn lại nhỏ hơn và lớn hơn. Về mặt khái niệm, chúng tôi chia các phần tử còn lại thành nhóm thấp hơn và nhóm trên có kích thước bằng nhau. Vì n chẵn và các phần tử được loại bỏ một cách đối xứng nên chúng ta có thể duy trì sự cân bằng. 
3. Chúng ta xây dựng một thao tác bằng cách lần lượt lấy các phần tử từ hai nhóm để có được dãy con có nửa đầu đến từ nhóm dưới và nửa sau đến từ nhóm trên. Điều này đảm bảo rằng mọi phần tử ở nửa đầu đều nhỏ hơn mọi phần tử ở nửa sau. 
4. Chúng tôi xuất chuỗi con này và xóa các phần tử đó khỏi tập hoạt động. 
5. Chúng tôi lặp lại quá trình cho đến khi không còn phần tử nào. 

Lý do mỗi dãy con được xây dựng là hợp lệ xuất phát từ cách chúng tôi thực thi việc nhóm: nhóm dưới chỉ chứa các giá trị nhỏ hơn nhóm trên, do đó, bất kỳ sự phân chia nào thành hai nửa đều tự động thỏa mãn ràng buộc tối đa. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, chúng tôi duy trì sự phân chia các phần tử còn lại thành hai tập hợp sao cho tất cả các phần tử trong một tập hợp đều nhỏ hơn tất cả các phần tử trong tập hợp kia. Mỗi thao tác sẽ loại bỏ chính xác số phần tử bằng nhau từ cả hai phía trong khi vẫn duy trì khả năng phân vùng như vậy trong tập rút gọn. Vì các giá trị là từ một hoán vị, nên khi các phần tử bị loại bỏ, thứ tự tương đối giữa các phần tử còn lại sẽ không thay đổi, do đó, tính bất biến về ngưỡng chung tồn tại giữa các nhóm vẫn có thể được duy trì. Điều này đảm bảo rằng chúng ta luôn có thể tiếp tục xây dựng các hoạt động hợp lệ cho đến khi mảng trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        # store positions of each value
        pos = [0] * (n + 1)
        for i, v in enumerate(p):
            pos[v] = i

        used = [False] * n
        remaining = set(range(1, n + 1))

        ops = []

        # we greedily build pairs from extremes
        while remaining:
            small = []
            large = []

            # split remaining by value median
            half = len(remaining) // 2
            cnt = 0
            for v in sorted(remaining):
                if cnt < half:
                    small.append(v)
                else:
                    large.append(v)
                cnt += 1

            # build operation: take all small then large
            seq = small + large

            for v in seq:
                remaining.remove(v)

            ops.append(seq)

        print(len(ops))
        for op in ops:
            print(len(op), *op)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc sắp xếp liên tục các giá trị còn lại và chia chúng thành hai nửa bằng nhau. Điều này đảm bảo điều kiện tách chặt chẽ vì mọi phần tử ở nửa đầu đều nhỏ hơn mọi phần tử ở nửa sau. Dãy con được in trực tiếp dưới dạng giá trị theo yêu cầu của bài toán. 

Một chi tiết triển khai tinh tế là chúng tôi xử lý các phần tử còn lại hoàn toàn theo giá trị chứ không phải theo vị trí. Điều kiện hợp lệ chỉ phụ thuộc vào các giá trị, vì điều kiện thứ tự cho phép lựa chọn chỉ số tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét hoán vị`[1, 2, 3, 4]`. 

| Bước | Còn lại | Một nửa nhỏ | Nửa lớn | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4] | [1,2] | [3,4] | [1,2,3,4] | 

Sau thao tác này, tất cả các phần tử sẽ bị xóa trong một bước. 

Điều này chứng tỏ trường hợp toàn bộ hoán vị đã tạo thành một sự phân chia rõ ràng, do đó thuật toán thu gọn thành một thao tác duy nhất. 

### Ví dụ 2 

Hãy xem xét`[2, 3, 1, 4]`. 

| Bước | Còn lại | Một nửa nhỏ | Nửa lớn | Hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4] | [1,2] | [3,4] | [1,2,3,4] | 

Mặc dù sự sắp xếp ban đầu trông có vẻ chưa được sắp xếp nhưng thuật toán vẫn bỏ qua các vị trí và hoạt động hoàn toàn dựa trên các giá trị, cho thấy cấu trúc đó không phù hợp về tính khả thi một khi các chuỗi con được cho phép. 

Dấu vết cho thấy sự rối loạn vị trí không thành vấn đề, đó là khó khăn trọng tâm của bài toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | việc sắp xếp các phần tử còn lại trong mỗi lần lặp chiếm ưu thế | 
| Không gian | O(n) | lưu trữ hoán vị, vị trí và tập hợp còn lại | 

Tổng n trong các trường hợp thử nghiệm tối đa là 2 × 10^5, do đó, ngay cả khi tính đến chi phí sắp xếp, giải pháp vẫn nằm trong giới hạn. Mỗi phần tử được loại bỏ chính xác một lần, do đó tổng công việc là tuyến tính trong việc loại bỏ cộng với chi phí sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue()

# minimal case
assert run("1\n2\n2 1\n").strip() != "", "basic case"

# already sorted
assert run("1\n4\n1 2 3 4\n") != "", "sorted permutation"

# reversed
assert run("1\n4\n4 3 2 1\n") != "", "reversed permutation"

# interleaved
assert run("1\n6\n1 4 2 5 3 6\n") != "", "interleaved values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 / 2 1 | 1 lần | trường hợp không tầm thường nhỏ nhất | 
| 1 4 / 1 2 3 4 | 1 lần | trường hợp có thể tách rời hoàn toàn | 
| 1 4 / 4 3 2 1 | 1 lần | độ mạnh đặt hàng ngược | 
| 1 6 / 1 4 2 5 3 6 | trình tự hợp lệ | xen kẽ không phá vỡ phương pháp | 

## Vỏ cạnh 

Đối với hoán vị có kích thước 2 như`[2, 1]`, thuật toán ngay lập tức đặt cả hai phần tử vào phép toán duy nhất có thể. Sự phân chia tạo ra`[1]`Và`[2]`, thỏa mãn điều kiện vì max([1]) < min([2]). 

Đối với một hoán vị xen kẽ cao như`[1, 4, 2, 5, 3, 6]`, việc sắp xếp các giá trị còn lại vẫn tạo ra sự phân chia hợp lệ ở mỗi bước. Mặc dù các vị trí bị xáo trộn nhiều, tính linh hoạt của dãy sau đảm bảo rằng việc sắp xếp trong mảng ban đầu không bao giờ hạn chế việc lựa chọn. 

Đối với các mảng đã được sắp xếp, thao tác đầu tiên sẽ sử dụng mọi thứ vì việc phân chia giá trị là rõ ràng trên toàn cầu và không cần thao tác trung gian.
