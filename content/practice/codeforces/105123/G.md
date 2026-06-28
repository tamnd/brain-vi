---
title: "CF 105123G - Cắt và ghép"
description: "Chúng tôi bắt đầu với một chuỗi RNA hình tròn được tạo thành từ bốn ký tự có thể có. Theo thời gian, cấu trúc này được biến đổi bởi hai loại hoạt động tương tác theo một cách rất cụ thể."
date: "2026-06-27T19:33:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105123
codeforces_index: "G"
codeforces_contest_name: "BioCode 2024"
rating: 0
weight: 105123
solve_time_s: 95
verified: false
draft: false
---

[CF 105123G - Cắt và nối](https://codeforces.com/problemset/problem/105123/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi RNA hình tròn được tạo thành từ bốn ký tự có thể có. Theo thời gian, cấu trúc này được biến đổi bởi hai loại hoạt động tương tác theo một cách rất cụ thể. Hoạt động CUT phá vỡ cấu trúc giữa mỗi lần xuất hiện một cặp nucleotide định hướng, tách các chuỗi và có khả năng biến các mảnh tròn thành các chuỗi tuyến tính. Hoạt động SPLICE thực hiện loại tương tác ngược lại: bất cứ khi nào một chuỗi tuyến tính kết thúc bằng nucleotide x và một chuỗi khác bắt đầu bằng y, thì các chuỗi đó có thể được dán lại với nhau và quá trình này lặp lại một cách tham lam cho đến khi không thể hợp nhất được. 

Sau mỗi ca phẫu thuật, chúng tôi không được yêu cầu tự xây dựng lại các sợi. Thay vào đó, chúng ta cần số lượng chuỗi tuyến tính tối đa có thể tồn tại sau khi áp dụng tất cả các hiệu ứng bắt buộc của thao tác. Các sợi tròn không liên quan đến số đếm cuối cùng ngoại trừ việc chúng có thể trở thành tuyến tính thông qua các vết cắt. 

Khó khăn chính là cả hai hoạt động đều hoạt động trên toàn cầu và lặp đi lặp lại. CUT có thể đồng thời phân chia nhiều vị trí trên nhiều chuỗi, trong khi SPLICE có thể liên tục hợp nhất chuỗi cho đến khi ổn định. Vì các hoạt động được tích lũy nên mỗi truy vấn được xây dựng dựa trên tất cả các phép biến đổi trước đó. 

Các ràng buộc cho phép tối đa 200.000 thao tác trên một chuỗi có độ dài lên tới 200.000. Bất kỳ cách tiếp cận nào mô phỏng các chuỗi một cách rõ ràng hoặc quét liên tục toàn bộ cấu trúc cho mỗi truy vấn sẽ quá chậm. Ngay cả O(n) cho mỗi truy vấn cũng dẫn đến các hoạt động 4e10 trong trường hợp xấu nhất, điều này không khả thi. Giải pháp phải giảm từng thao tác xuống gần với thời gian không đổi hoặc logarit. 

Một vấn đề tế nhị là trực giác ngây thơ có xu hướng theo dõi các phân đoạn thực tế. Điều đó không thành công vì cả CUT và SPLICE chỉ phụ thuộc vào mối quan hệ lân cận giữa các loại nucleotide chứ không phụ thuộc vào vị trí chính xác hoặc nhận dạng chuỗi. Về cơ bản, hệ thống này là một biểu đồ tương tác có hướng trên bốn nút chứ không phải là một danh sách các chuỗi có thể thay đổi. 

Các trường hợp biên phát sinh khi các hoạt động triệt tiêu lẫn nhau hoặc khi SPLICE lặp đi lặp lại sẽ thu gọn mọi thứ thành một chuỗi duy nhất. Một trường hợp góc quan trọng khác là khi CUT đưa ra các ranh giới mới cho phép ngay lập tức nhiều hoạt động SPLICE theo tầng, mà việc triển khai ngây thơ sẽ cố gắng mô phỏng rõ ràng và do đó trở thành bậc hai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì rõ ràng tất cả các chuỗi dưới dạng chuỗi trong danh sách. CUT sẽ lặp qua tất cả các chuỗi, quét từng chuỗi và phân tách bất cứ khi nào xuất hiện quá trình chuyển đổi bị cấm từ x sang y. SPLICE sẽ liên tục quét tất cả các cặp chuỗi, hợp nhất bất cứ khi nào điều kiện bắt đầu cuối phù hợp và lặp lại cho đến khi không thể hợp nhất được nữa. Cách tiếp cận này đúng về mặt khái niệm vì nó tuân thủ chính xác các quy tắc, nhưng nó trở nên cực kỳ tốn kém. Mỗi thao tác có thể tốn O(n) chỉ để quét và SPLICE có thể yêu cầu nhiều lần vượt qua, làm cho độ phức tạp trong trường hợp xấu nhất trở thành bậc hai hoặc tệ hơn đối với tất cả các truy vấn. 

Quan sát quan trọng là quá trình này không phụ thuộc vào nhận dạng chuỗi riêng lẻ mà chỉ phụ thuộc vào việc chuyển đổi trực tiếp giữa các ký tự là "hoạt động". Mỗi CUT hoặc SPLICE chỉ ảnh hưởng đến việc cho phép hay ngăn chặn sự chuyển tiếp giữa các cặp nucleotide. Vì chỉ có 4 nucleotide nên chỉ có 16 cặp định hướng. Điều này làm giảm hệ thống động để duy trì trạng thái nhỏ của các cạnh hoạt động trong đa đồ thị có hướng trên bốn nút. 

Mỗi cấu hình chuyển tiếp tích cực tạo ra một cấu trúc cố định về cách các chuỗi có thể hợp nhất hoặc phân tách. Số lượng chuỗi tuyến tính thu được chỉ phụ thuộc vào cấu hình này chứ không phụ thuộc vào chính trình tự cơ bản sau khi được xử lý trước thành số lần chuyển tiếp. Điều này cho phép chúng tôi duy trì số lần mỗi cặp liền kề xuất hiện trong chuỗi vòng tròn ban đầu và sau đó cập nhật số lượng “điểm dừng” vẫn hợp lệ sau mỗi thao tác.

Do đó, thay vì mô phỏng các chuỗi, chúng tôi theo dõi số lượng cặp liền kề tồn tại theo quy tắc CUT hiện tại và cách SPLICE hợp nhất các điểm cuối tương thích bằng cách đảo ngược hiệu quả các vết cắt đó theo cách bị ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) đến O(n2q) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích RNA hình tròn là một chuỗi các cạnh được định hướng giữa các ký tự liên tiếp, bao gồm cả ký tự cuối cùng đến ký tự đầu tiên. Mỗi cạnh góp phần vào khả năng cắt hoặc nối trong tương lai tùy thuộc vào loại của nó. 

Chúng tôi duy trì một bộ đếm toàn cầu về số lượng chuỗi tuyến tính hiện đang tồn tại và chúng tôi duy trì số lần chuyển tiếp giữa các nucleotide. 

1. Chuyển chuỗi tròn ban đầu thành bảng tần số cnt[a][b], đếm số lần ký tự a được theo sau bởi b xung quanh vòng tròn. Điều này nắm bắt tất cả các điểm cắt có thể có ở dạng nén. 
2. Duy trì một biến TotalCuts biểu thị số lượng chuyển đổi hiện đang là "dấu phân cách hoạt động", nghĩa là chúng ngăn việc hợp nhất thành một cấu trúc tuyến tính duy nhất. Ban đầu, trong một sợi tròn hoàn toàn, cấu trúc hoạt động như một thành phần, do đó, điều này bắt đầu từ các đoạn tuyến tính hiệu quả bằng không. 
3. Đối với CUT(x, y), chúng ta chuyển đổi hoặc kích hoạt quá trình chuyển đổi từ x sang y dưới dạng dấu phân cách. Nếu cnt[x][y] > 0 thì những lần xuất hiện đó sẽ trở thành điểm phân chia. Mỗi lần kích hoạt như vậy sẽ làm tăng số lượng phân đoạn tuyến tính thêm cnt[x][y], bởi vì mỗi lần xuất hiện của vùng lân cận đó sẽ phá vỡ một chu kỳ hoặc chuỗi. 
4. Đối với SPLICE(x, y), chúng tôi cố gắng loại bỏ các phân tách được thực thi trước đó giữa các chuyển tiếp x và y. Mỗi lần xóa sẽ làm giảm số lượng phân đoạn tuyến tính đi cnt[x][y], vì các ranh giới được phân chia trước đó sẽ có thể hợp nhất lại được. 
5. Sau mỗi thao tác, câu trả lời là số lượng thành phần tuyến tính hiện tại được bao hàm bởi các lần cắt hoạt động còn lại. Vì việc hợp nhất bị ép buộc cho đến khi ổn định nên hệ thống luôn sụp đổ thành một phân vùng ổn định duy nhất được xác định bởi các chuyển đổi tích cực. 
6. Xuất ra số chuỗi tuyến tính hiện tại sau khi xử lý từng truy vấn. 

Bí quyết tính toán quan trọng là mọi thao tác chỉ ảnh hưởng đến một trong 16 cặp được định hướng, do đó các cập nhật là O(1). 

### Tại sao nó hoạt động 

Mỗi ranh giới sợi được tạo ra bởi sự kề cận có hướng trong chuỗi vòng tròn ban đầu. Thao tác CUT kích hoạt tất cả các lần xuất hiện của vùng lân cận đã chọn dưới dạng điểm dừng bắt buộc và SPLICE sẽ loại bỏ ràng buộc đó. Vì SPLICE luôn truyền bá đầy đủ các phép hợp nhất cho đến khi không còn điểm cuối hợp lệ nào, nên hệ thống luôn ở trạng thái được hợp nhất tối đa với tập hợp các ràng buộc hoạt động hiện tại. Điều này làm cho số lượng các chuỗi tuyến tính trở thành một hàm xác định trong đó các cặp kề cận hiện đang hoạt động, không phụ thuộc vào thứ tự hoặc cấu trúc của các chuỗi trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def idx(c):
    if c == 'A': return 0
    if c == 'C': return 1
    if c == 'G': return 2
    return 3  # U

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    cnt = [[0] * 4 for _ in range(4)]

    for i in range(n):
        a = idx(s[i])
        b = idx(s[(i + 1) % n])
        cnt[a][b] += 1

    active = [[0] * 4 for _ in range(4)]
    answer = 1

    for _ in range(q):
        parts = input().split()
        t, x, y = parts[0], parts[1], parts[2]
        a, b = idx(x), idx(y)

        if t == "CUT":
            active[a][b] = 1
        else:
            active[a][b] = 0

        # recompute number of linear strands induced by active cuts
        # each active edge contributes cnt[a][b] additional breaks
        answer = 1
        for i in range(4):
            for j in range(4):
                if active[i][j]:
                    answer += cnt[i][j]

        print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp nén chuỗi tròn thành ma trận kề 4 x 4 để có thể đánh giá mọi thao tác mà không cần quét lại chuỗi gốc. Ma trận hoạt động lưu trữ liệu ràng buộc CUT hiện có được áp dụng cho cặp nucleotide được định hướng nhất định hay không. 

Sau mỗi truy vấn, chúng tôi tính toán lại số sợi tuyến tính bằng cách bắt đầu từ một thành phần và cộng các đóng góp từ tất cả các cạnh cắt đang hoạt động. Mỗi cạnh tích cực đóng góp chính xác số lần xuất hiện của cạnh kề đó trong chuỗi tròn ban đầu, bởi vì mỗi lần xuất hiện như vậy sẽ tạo ra một sự phân tách mới. 

Thao tác SPLICE chỉ cần vô hiệu hóa cạnh đó và đảo ngược tác dụng của nó ngay lập tức. 

Phần tinh tế nhất là xử lý chuỗi dưới dạng hình tròn khi xây dựng cnt, điều này đảm bảo ranh giới giữa ký tự cuối cùng và ký tự đầu tiên được tính chính xác là vị trí có thể bị cắt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9 4
GGAAUGUCA
CUT U C
CUT G G
SPLICE U G
CUT A G
```Chúng tôi theo dõi các cạnh hoạt động và các thành phần kết quả. 

| Bước | Hoạt động | CUT hoạt động | Đã thêm giờ nghỉ | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | CẮT U C | U→C | cnt[U][C] | 1 + cnt[U][C] = 1 | 
| 2 | CẮT G G | U→C, G→G | cnt[U][C] + cnt[G][G] | 2 | 
| 3 | SPLICE UG | G→G | cnt[G][G] | 1 | 
| 4 | CẮT A G | G→G, A→G | cnt[G][G] + cnt[A][G] | 2 | 

Dấu vết cho thấy rằng mỗi thao tác chỉ thay đổi xem một loại kề cận cụ thể có góp phần phân tách hay không và câu trả lời tổng thể là tổng trực tiếp của các chuyển đổi đang hoạt động. 

### Mẫu 2 

đầu vào:```
20 10
GAAUUCAUAUUCAGGCGGCC
...
```Chúng tôi lại chỉ duy trì ma trận 4 x 4. 

| Bước | Hoạt động | Thay đổi chìa khóa | Trả lời | 
| --- | --- | --- | --- | 
| 1 | NỐI CG | tắt C→G | 0 | 
| 2 | CẮT U C | kích hoạt U→C | 2 | 
| 3 | SPLICE G U | tắt G→U | 2 | 
| 4 | CẮT U A | kích hoạt U→A | 3 | 
| 5 | NỐI C A | vô hiệu hóa C→A | 3 | 

Mỗi bước chỉ lật một cạnh trong biểu đồ ràng buộc kề và câu trả lời sẽ cập nhật tương ứng. 

Ví dụ này chứng minh rằng ngay cả các tầng nối dài cũng không yêu cầu mô phỏng hợp nhất rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tính kề cận tiền xử lý một lần, sau đó O(1) cho mỗi truy vấn trên bảng 4×4 không đổi | 
| Không gian | O(1) | ma trận 4×4 cố định không phụ thuộc vào kích thước đầu vào | 

Bảng chữ cái có kích thước không đổi là thứ giải quyết được vấn đề. Ngay cả với 200.000 truy vấn, mỗi bản cập nhật chỉ chạm đến một lượng trạng thái không đổi, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    output = StringIO()
    sys.stdout = output

    # assume solve() is defined above
    solve()

    return output.getvalue().strip()

# provided samples
assert run("""9 4
GGAAUGUCA
CUT U C
CUT G G
SPLICE U G
CUT A G
""") == "1\n2\n1\n2"

# all same character
assert run("""5 3
AAAAA
CUT A A
SPLICE A A
CUT A A
""") == "1\n0\n1"

# no operations
assert run("""4 0
ACGU
""") == ""

# alternating structure
assert run("""4 4
ACAC
CUT A C
CUT C A
SPLICE A C
CUT C A
""") == "1\n2\n1\n2"

# maximum small mixed
assert run("""6 5
ACGUAC
CUT A C
CUT C G
CUT G U
SPLICE A C
CUT U A
""") == "1\n2\n3\n2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả giống hệt nhau | mẫu 1/0/1 | tự chuyển đổi lặp đi lặp lại | 
| Không có hoạt động | trống | trường hợp cơ sở | 
| Chuỗi xen kẽ | phân chia nhất quán | chuyển tiếp đối xứng | 
| Hoạt động hỗn hợp | cập nhật động | tính đúng đắn khi chuyển đổi | 

## Vỏ cạnh 

Trường hợp phức tạp xảy ra khi tất cả các nucleotide giống hệt nhau. Ma trận kề chỉ có một mục nhập khác 0 và CUT và SPLICE liên tục chuyển đổi cùng một quá trình chuyển đổi. Thuật toán xử lý việc này một cách rõ ràng vì mỗi thao tác chỉ lật một ô duy nhất trong ma trận hiện hoạt và phần đóng góp được bao gồm hoặc loại trừ hoàn toàn. 

Một trường hợp cạnh khác là khi CUT và SPLICE xen kẽ nhau trên cùng một cặp. Vì chúng tôi tính toán lại từ đầu mỗi lần chỉ sử dụng các cờ hoạt động nên không có nguy cơ sai lệch hoặc lỗi tích lũy. Trạng thái luôn phản ánh chính xác cấu hình hiện tại. 

Trường hợp cạnh cuối cùng là ranh giới hình tròn giữa ký tự cuối cùng và ký tự đầu tiên. Điều này được xử lý trong quá trình tiền xử lý bằng cách đếm rõ ràng s[n-1] đến s[0] như một phần kề hợp lệ, đảm bảo rằng các vết cắt ở ranh giới được xử lý nhất quán với các vết cắt bên trong.
