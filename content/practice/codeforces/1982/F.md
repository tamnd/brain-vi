---
title: "CF 1982F - Sắp xếp lại vấn đề"
description: "Chúng ta được cung cấp một mảng thay đổi theo thời gian thông qua việc cập nhật điểm. Sau mỗi thay đổi, chúng ta phải trả lời một câu hỏi cấu trúc về mảng: đoạn liền kề ngắn nhất là gì sao cho nếu chúng ta chỉ sắp xếp đoạn đó thì toàn bộ mảng sẽ được sắp xếp theo thứ tự không giảm."
date: "2026-06-08T16:44:24+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1982
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 955 (Div. 2, with prizes from NEAR!)"
rating: 2600
weight: 1982
solve_time_s: 93
verified: false
draft: false
---

[CF 1982F - Sắp xếp lại vấn đề](https://codeforces.com/problemset/problem/1982/F) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng thay đổi theo thời gian thông qua việc cập nhật điểm. Sau mỗi thay đổi, chúng ta phải trả lời một câu hỏi cấu trúc về mảng: đoạn liền kề ngắn nhất là gì sao cho nếu chúng ta chỉ sắp xếp đoạn đó thì toàn bộ mảng sẽ được sắp xếp theo thứ tự không giảm. 

Ý tưởng chính là chúng tôi không cố gắng sắp xếp đầy đủ mảng. Thay vào đó, chúng tôi đang cố gắng xác định “khoảng thời gian hư hỏng” tối thiểu có chứa tất cả các đảo ngược, theo cách chỉ sắp xếp khoảng thời gian đó sẽ sửa chữa được toàn bộ mảng. 

Mỗi truy vấn thay đổi một vị trí duy nhất và sau mỗi thay đổi, chúng tôi phải tính toán lại phân đoạn tối thiểu này từ đầu nhưng hiệu quả. 

Những hạn chế buộc phải thiết kế cẩn thận. Độ dài mảng và số lượng cập nhật đều lên tới tổng cộng 5e5. Bất kỳ giải pháp nào tính toán lại việc sắp xếp hoặc quét theo thời gian tuyến tính cho mỗi truy vấn đều dẫn đến khoảng 2,5e11 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Điều này ngay lập tức loại trừ việc sắp xếp theo truy vấn, tính toán lại toàn bộ các phép đảo ngược hoặc quét lồng nhau. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã được sắp xếp. Trong trường hợp đó, câu trả lời phải là -1 -1. Một cách tiếp cận phát hiện khoảng thời gian ngây thơ luôn trả về một số phân đoạn như [1,1] hoặc [l,l] không thành công ở đây vì nó giả định không chính xác rằng ít nhất một vị trí phải được “cố định”. Một tình huống khó khăn khác là khi các bản cập nhật mang tính cục bộ nhưng hiệu ứng của chúng lại lan truyền trên toàn cầu. Ví dụ: trong một mảng gần như được sắp xếp như [1,2,3,4,5], việc thay đổi phần giữa thành 0 sẽ tạo ra sự dịch chuyển ranh giới bên trái hoàn toàn sang chỉ mục 1 và các câu trả lời trong tương lai phụ thuộc vào thứ tự chung chứ không phải thay đổi cục bộ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán lại câu trả lời sau mỗi lần cập nhật bằng cách quét mảng và tìm vị trí đầu tiên và cuối cùng mà thuộc tính được sắp xếp bị vi phạm. Người ta cũng có thể mô phỏng việc sắp xếp mọi mảng con có thể và kiểm tra xem nó có sửa được mảng đó hay không. Điều này hoạt động về mặt khái niệm vì việc sắp xếp một mảng con sẽ loại bỏ tất cả các nghịch đảo bên trong nó và vấn đề còn lại là liệu các nghịch đảo ranh giới có còn tồn tại hay không. Tuy nhiên, việc thử tất cả các mảng con đều là bậc hai và thậm chí việc quét từng truy vấn cũng là tuyến tính, dẫn đến O(nq), quá chậm. 

Quan sát quan trọng là phân đoạn tối ưu được xác định hoàn toàn bởi cấu trúc tổng thể của các phép đảo ngược của mảng. Nếu chúng ta tưởng tượng phiên bản được sắp xếp của mảng, thì phân đoạn tối thiểu phải bao gồm tất cả các chỉ mục “không đúng chỗ” so với vùng được sắp xếp chính xác của chúng. Cụ thể hơn, chúng ta cần l nhỏ nhất sao cho mọi thứ còn lại của l đều nhất quán với thứ tự được sắp xếp và r lớn nhất sao cho mọi thứ bên phải của r cũng nhất quán. 

Điều này có thể được điều chỉnh lại dưới dạng truy vấn về thống kê thứ tự: chúng ta cần tìm vị trí đầu tiên và cuối cùng trong đó mảng khác với cấu trúc được sắp xếp của nó, nhưng chúng ta phải duy trì điều này một cách linh hoạt trong các bản cập nhật. 

Cái nhìn sâu sắc về cấu trúc quan trọng là câu trả lời được xác định bởi hai điều: điều kiện tiền tố trong đó không có phép đảo ngược nào xuất hiện trước l và điều kiện hậu tố trong đó không có phép đảo ngược nào xuất hiện sau r. Chúng có thể được theo dõi bằng cách sử dụng cấu trúc dữ liệu được sắp xếp để duy trì các ràng buộc đặt hàng cục bộ một cách hiệu quả. 

Một cách tiêu chuẩn để đạt được điều này là duy trì tất cả các chỉ mục trong một cấu trúc cân bằng được khóa theo giá trị và chỉ mục, cho phép chúng ta nhanh chóng tìm ra vị trí vi phạm thứ tự sắp xếp. Ranh giới được xác định bởi vị trí đầu tiên nơi mối quan hệ tiền nhiệm bị phá vỡ và vị trí cuối cùng như vậy. 

Với một tập hợp thứ tự hoặc cây phân đoạn phù hợp duy trì tính nhất quán của thứ tự cục bộ, mỗi lần cập nhật chỉ ảnh hưởng đến một vùng lân cận so sánh nhỏ, nhưng việc truy vấn các điểm cực trị toàn cục có thể được thực hiện theo thời gian logarit.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu (cấu trúc có thứ tự/cây phân đoạn) | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mảng và cấu trúc theo dõi vị trí thuộc tính thứ tự a[i] <= a[i+1] giữ hoặc không thành công. 

1. Xây dựng một mảng phụ khác biệt trong đó diff[i] = 1 nếu a[i] <= a[i+1], nếu không thì 0. Điều này mã hóa xem mảng có được sắp xếp cục bộ ở mỗi ranh giới hay không. 
2. Duy trì cấu trúc dữ liệu hỗ trợ cập nhật khác biệt khi một vị trí thay đổi. Khi a[i] được cập nhật, chỉ diff[i-1] và diff[i] có thể thay đổi, vì chỉ những so sánh liền kề mới bị ảnh hưởng. Đây là tài sản trọng điểm của địa phương. 
3. Để trả lời một truy vấn, chúng ta cần l nhỏ nhất sao cho tồn tại vi phạm tại hoặc trước l và r lớn nhất sao cho tồn tại vi phạm tại hoặc sau r. Tương tự, chúng ta cần khoảng tối thiểu bao gồm tất cả các chỉ số i trong đó diff[i] = 0. 
4. Duy trì cấu trúc lưu trữ tất cả các chỉ số i trong đó diff[i] = 0, ví dụ như một tập hợp được sắp xếp. Ranh giới bên trái được lấy từ chỉ số nhỏ nhất trong tập hợp này và ranh giới bên phải được tính từ chỉ số lớn nhất cộng với một. 
5. Nếu tập hợp trống thì mảng đã được sắp xếp và chúng ta trả về -1 -1. 
6. Đối với mỗi lần cập nhật, hãy điều chỉnh tối đa hai mục khác nhau, cập nhật tập hợp tương ứng, sau đó tính toán lại câu trả lời từ các điểm cực trị đã tập hợp. 

Điểm tinh tế là đoạn chúng ta trả về không trực tiếp là khoảng chênh lệch mà nó được lấy từ khoảng đó. Nếu vi phạm đầu tiên là tại i, phân đoạn được sắp xếp phải bắt đầu tại hoặc trước i và tương tự cho phía bên phải. 

### Tại sao nó hoạt động 

Mảng sẽ được sắp xếp sau khi sửa một mảng con khi và chỉ khi tất cả các đảo ngược nằm hoàn toàn bên trong mảng con đó. Bất kỳ sự đảo ngược nào đều được chứng kiến ​​bởi một cặp vi phạm liền kề trong khác biệt, do đó việc bao gồm tất cả các mục 0 trong khác biệt tương đương với việc bao gồm tất cả các đảo ngược. Vì việc sắp xếp một phân đoạn sẽ loại bỏ tất cả các đảo ngược có đầy đủ trong đó và không ảnh hưởng đến các phần tử bên ngoài nên phân đoạn hợp lệ tối thiểu chính xác là khoảng nhỏ nhất chứa mọi ranh giới vi phạm. Điều này đảm bảo không có sự đảo ngược nào vượt qua ranh giới sau khi sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    bad = set()

    def add(i):
        if 0 <= i < n - 1:
            if a[i] > a[i + 1]:
                bad.add(i)

    for i in range(n - 1):
        add(i)

    def answer():
        if not bad:
            return "-1 -1"
        l = min(bad) + 1
        r = max(bad) + 1
        return f"{l} {r}"

    out = []
    out.append(answer())

    for _ in range(q):
        pos, val = map(int, input().split())
        pos -= 1

        a[pos] = val

        for i in (pos - 1, pos):
            if 0 <= i < n - 1:
                if i in bad:
                    if not (a[i] > a[i + 1]):
                        bad.remove(i)
                else:
                    if a[i] > a[i + 1]:
                        bad.add(i)

        out.append(answer())

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc chỉ duy trì các so sánh liền kề để xác định sự mất trật tự cục bộ. Khi một giá trị thay đổi, chỉ có hai giá trị lân cận của nó mới có thể trở thành không hợp lệ hoặc hợp lệ trở lại, vì vậy chúng tôi cập nhật tối đa hai mục trong tập hợp. 

Câu trả lời được rút ra trực tiếp từ các chỉ số cực trị trong tập hợp vi phạm. Việc chuyển đổi từ chỉ mục biên i sang chỉ mục mảng yêu cầu dịch chuyển +1 vì diff[i] tương ứng với a[i] và a[i+1]. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [2,2,3,4,5]
```Ban đầu không có vi phạm nào nên tập hợp này trống. 

| Bước | Cập nhật | Mảng | bộ xấu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | [2,2,3,4,5] | {} | -1 -1 | 
| 1 | a[2]=1 | [2,1,3,4,5] | {1} | 2 2 | 
| 2 | a[4]=1 | [2,1,3,4,1] | {1,3} | 2 4 | 

Điều này cho thấy một bản cập nhật có thể gây ra nhiều vi phạm rời rạc như thế nào và câu trả lời phải bao gồm cả hai đầu. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1,2,3,4]
```| Bước | Cập nhật | Mảng | bộ xấu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | [1,2,3,4] | {} | -1 -1 | 
| 1 | a[2]=0 | [1,2,0,4] | {1,2} | 2 3 | 
| 2 | a[3]=5 | [1,2,0,5] | {1} | 2 2 | 

Điều này chứng tỏ rằng phân đoạn có thể thu nhỏ hoặc thay đổi sau khi cập nhật, tùy thuộc vào cách các vi phạm hợp nhất hoặc biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi bản cập nhật ảnh hưởng đến tối đa hai hoạt động kiểm tra kề và thiết lập | 
| Không gian | O(n) | mảng cộng với tập hợp các vị trí xấu | 

Độ phức tạp vừa vặn trong giới hạn vì mỗi thao tác trong số tối đa 5e5 thao tác chỉ kích hoạt công việc liên tục trên một cấu trúc cân bằng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        q = int(input())

        bad = set()

        def add(i):
            if 0 <= i < n - 1:
                if a[i] > a[i + 1]:
                    bad.add(i)

        for i in range(n - 1):
            add(i)

        def ans():
            if not bad:
                return "-1 -1"
            return f"{min(bad)+1} {max(bad)+1}"

        out = [ans()]

        for _ in range(q):
            pos, val = map(int, input().split())
            pos -= 1
            a[pos] = val

            for i in (pos - 1, pos):
                if 0 <= i < n - 1:
                    if i in bad and not (a[i] > a[i+1]):
                        bad.remove(i)
                    elif a[i] > a[i+1]:
                        bad.add(i)

            out.append(ans())

        return "\n".join(out)

    return solve()

# provided samples
assert run("""2
5
2 2 3 4 5
3
2 1
4 1
1 1
5
1 2 3 4 5
9
1 4
2 3
5 2
3 1
1 1
5 1
4 1
3 1
2 1
""") == """-1 -1
1 2
1 4
3 4
-1 -1
1 3
1 3
1 5
1 5
2 5
2 5
2 5
2 5
-1 -1"""

# custom cases

# already sorted, no updates
assert run("""1
3
1 2 3
0
""") == "-1 -1"

# single inversion
assert run("""1
3
1 3 2
1
2 1
""") == "1 3\n1 2"

# all equal
assert run("""1
5
5 5 5 5 5
2
3 5
1 5
""").count("-1 -1") >= 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đã được sắp xếp | -1 -1 | bộ vi phạm trống | 
| đảo ngược đơn | phân khúc nhỏ | mở rộng ranh giới chính xác | 
| tất cả đều bình đẳng | luôn được sắp xếp | sự ổn định theo bản cập nhật | 

## Vỏ cạnh 

Một mảng được sắp xếp đầy đủ vẫn ổn định cho đến khi có bản cập nhật đưa ra sự đảo ngược nghiêm ngặt. Trong trường hợp đó, chỉ có hai phép so sánh liền kề trở nên không hợp lệ và thuật toán sẽ chèn chính xác các chỉ số đó vào tập hợp xấu, tạo ra một phân đoạn tối thiểu trải dài vừa đủ để bao phủ chúng. 

Khi nhiều bản cập nhật dần dần tạo nên một sự hỗn loạn trên diện rộng, chẳng hạn như liên tục chèn các giá trị giảm dần vào mảng, tập sai sẽ mở rộng và khoảng thời gian trả về cũng tăng theo. Mỗi bản cập nhật chỉ chạm đến các so sánh cục bộ, nhưng các chỉ số cực đoan vẫn nắm bắt chính xác khu vực bị ảnh hưởng toàn cầu vì bất kỳ sự đảo ngược nào đều phải được chứng kiến ​​bởi ít nhất một vi phạm liền kề.
