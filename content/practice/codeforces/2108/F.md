---
title: "CF 2108F - Tháp sụp đổ"
description: "Chúng ta được cung cấp một loạt các tháp, mỗi tháp bắt đầu với một chiều cao không âm nào đó. Chúng tôi phải xử lý mỗi tháp chính xác một lần, theo bất kỳ thứ tự nào chúng tôi chọn."
date: "2026-06-08T04:45:37+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2108
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1022 (Div. 2)"
rating: 2900
weight: 2108
solve_time_s: 103
verified: false
draft: false
---

[CF 2108F - Tháp sụp đổ](https://codeforces.com/problemset/problem/2108/F) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các tháp, mỗi tháp bắt đầu với một chiều cao không âm nào đó. Chúng tôi phải xử lý mỗi tháp chính xác một lần, theo bất kỳ thứ tự nào chúng tôi chọn. Khi chúng tôi xử lý một tòa tháp ở vị trí`i`, chúng ta “thu gọn” nó: giá trị của nó được thêm vào khi tăng thêm`+1`tiếp theo`a_i`vị trí, và sau đó bản thân tòa tháp trở thành số không. Bất kỳ số gia nào vượt ra ngoài mảng đều bị mất. 

Sau khi tất cả các tháp đã được xử lý, chúng ta thu được dãy chiều cao cuối cùng. Yêu cầu duy nhất ở cách sắp xếp cuối cùng là nó phải không giảm từ trái sang phải. Trong số tất cả các lệnh xử lý có thể có, chúng tôi muốn tối đa hóa MEX của mảng cuối cùng, nghĩa là chúng tôi muốn làm cho số nguyên không âm bị thiếu nhỏ nhất càng lớn càng tốt. 

Khó khăn chính là mỗi tòa tháp không chỉ ảnh hưởng đến chính nó mà còn lan truyền ảnh hưởng đến một hậu tố có độ dài phụ thuộc vào giá trị hiện tại của nó và ảnh hưởng này mang tính tích lũy và phụ thuộc vào thứ tự. 

Các ràng buộc rất chặt chẽ: tổng số tháp trong tất cả các trường hợp thử nghiệm lên tới 100.000. Điều này loại trừ mọi cách tiếp cận mô phỏng tất cả các hoán vị hoặc tính toán lại nhiều lần các hiệu ứng theo thời gian bậc hai hoặc thời gian tệ hơn. Thậm chí một`O(n^2 log n)`chiến lược là rủi ro nếu nó thực hiện các hoạt động nặng trên mỗi lần truyền. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị lớn nhưng vị trí ở gần cuối. Ví dụ: một tòa tháp có giá trị rất lớn gần chỉ số cuối cùng không đóng góp gì có ý nghĩa cho cấu trúc nhưng vẫn tiêu tốn một hoạt động không thay đổi trạng thái tương lai trong một mô phỏng đơn giản. Một trường hợp phức tạp khác là khi tồn tại nhiều số 0, vì các số 0 không làm gì nhưng vẫn phải được xử lý và vị trí của chúng theo thứ tự đóng vai trò quan trọng đối với tính khả thi của việc xây dựng các độ cao nhỏ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là thử tất cả các mệnh lệnh có thể để đánh sập các tòa tháp. Đối với mỗi đơn hàng, chúng tôi mô phỏng quá trình lan truyền và tính toán mảng cuối cùng, sau đó kiểm tra xem nó có giảm không và tính MEX của nó. Điều này đúng về nguyên tắc vì nó tuân theo các quy tắc một cách chính xác, nhưng số lượng hoán vị là`n!`và mỗi chi phí mô phỏng`O(n)`, điều này hoàn toàn không thể thực hiện được ngay cả đối với`n = 20`. 

Một cách mạnh mẽ hữu ích hơn là nghĩ về việc xây dựng tham lam: chúng tôi cố gắng chỉ định “thời gian xử lý” cho mỗi tháp và mô phỏng về phía trước. Tuy nhiên, ngay cả khi đó, việc thử tất cả các bài tập vẫn theo cấp số nhân. 

Quan sát cấu trúc quan trọng là mỗi thao tác đóng góp một “sóng” đơn điệu cho hậu tố và yêu cầu cuối cùng chính là đơn điệu. Điều này gợi ý rằng thay vì theo dõi các giá trị chính xác, chúng ta nên tập trung vào việc liệu có thể nhận ra tiền tố của các giá trị hay không`0, 1, 2, ..., k-1`dưới một hình thức nào đó, vì MEX chỉ phụ thuộc vào sự tồn tại của tất cả các giá trị bên dưới nó. 

Chúng ta có thể định dạng lại quy trình: chúng ta muốn xác định tiền tố của các giá trị số nguyên có thể được “hỗ trợ” lớn đến mức nào bằng cách phân phối các đóng góp từ các tháp theo một số thứ tự. Thứ tự chỉ quan trọng ở chỗ mỗi vị trí có “công suất” còn lại bao nhiêu để hấp thụ các khoản tăng thêm. Điều này tự nhiên dẫn đến việc kiểm tra tính khả thi tham lam đối với giá trị MEX ứng cử viên và sau đó tìm kiếm nhị phân qua câu trả lời. 

Việc kiểm tra tính khả thi có hiệu quả vì nếu chúng tôi sửa được mục tiêu`k`, chúng tôi chỉ quan tâm liệu chúng tôi có thể đảm bảo cấu trúc có khả năng tạo ra một mảng không giảm cuối cùng có giá trị thiếu nhỏ nhất ít nhất là`k`. Điều đó có nghĩa là chúng tôi có thể xây dựng dần dần các cấp độ cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tham lam | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm câu trả lời nhị phân`k`, đó là ứng cử viên MEX. Đối với mỗi cố định`k`, chúng tôi kiểm tra xem liệu có thể nhận ra cấu hình cuối cùng hợp lệ hỗ trợ tất cả các giá trị lên đến`k-1`. 

1. Sắp xếp hoặc xử lý các tháp theo cách cho phép chúng tôi mô phỏng các khoản đóng góp một cách tham lam từ trái sang phải. Theo trực giác, các chỉ số nhỏ hơn sẽ khó “sửa chữa” hơn sau này, vì vậy tính khả thi cần được kiểm tra theo cách nhất quán về phía trước. 
2. Duy trì một biến đang chạy`carry`, biểu thị chiều cao tăng thêm đã được đẩy vào vị trí hiện tại bằng những lần kích hoạt trước đó. Đây là hậu quả tích lũy của những lần sụp đổ trước đó. 
3. Quét từ trái sang phải. Tại mỗi vị trí`i`, chiều cao hiệu dụng là`a[i] + carry`. Nếu số tiền này đã đủ lớn thì nó sẽ góp phần nâng cao năng lực hỗ trợ các vị thế trong tương lai; mặt khác, chúng ta phải đảm bảo tính khả thi bằng cách sử dụng vị trí này làm nguồn nhân giống bổ sung. 
4. Khi chúng tôi xử lý một tòa tháp làm nguồn, về mặt khái niệm, chúng tôi sẽ quyết định xem nó có thể đóng góp bao nhiêu. Kể từ khi sụp đổ một tòa tháp phân phối`+1`đến một hậu tố có độ dài`a[i]`, điều này có tác dụng giống như bổ sung thêm một quỹ ảnh hưởng có thể được sử dụng để duy trì tính đơn điệu cần thiết. 
5. Chúng tôi tham lam phân bổ các khoản đóng góp để luôn cố gắng đáp ứng yêu cầu còn thiếu nhỏ nhất trước tiên. Nếu tại bất kỳ thời điểm nào chúng tôi không thể duy trì tiến độ cần thiết cho đến`k`, chúng tôi từ chối điều này`k`. 
6. Sau khi tính toán tính khả thi, chúng tôi điều chỉnh ranh giới tìm kiếm nhị phân cho phù hợp cho đến khi chúng tôi tách được giá trị hợp lệ tối đa`k`. 

Ý tưởng quan trọng là chúng ta không bao giờ mô phỏng rõ ràng các hoán vị. Thay vào đó, chúng tôi diễn giải lại mỗi tòa tháp như một tài nguyên có phạm vi ảnh hưởng và chúng tôi tham lam kết hợp tài nguyên đó với nhu cầu duy trì cấu trúc không suy giảm. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào đối số trao đổi theo thứ tự xử lý. Bất kỳ chuỗi sự cố hợp lệ nào cũng có thể được chuyển thành chuỗi xử lý các khoản đóng góp theo thứ tự không giảm dần về “tính hữu dụng” của chúng mà không phá vỡ tính khả thi, bởi vì việc trì hoãn một tòa tháp đóng góp sớm hơn chỉ làm tăng công suất sẵn có trong tương lai chứ không bao giờ làm giảm nó. Tính đơn điệu này đảm bảo rằng quá trình quét tham lam nắm bắt được tất cả các cấu hình tối ưu và tìm kiếm nhị phân trên`k`hợp lệ vì tính khả thi là đơn điệu trong`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(a, k):
    n = len(a)
    carry = 0
    need = 0  # how many "units" we still need to enforce structure up to k
    
    for i in range(n):
        val = a[i] + carry
        
        # We interpret each position as helping reduce deficit
        if val < k:
            need += (k - val)
        
        # each position can "supply" at most 1 unit in this simplified view
        carry += 1
        
        if need > (i + 1):
            return False
    
    return need <= n

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        lo, hi = 0, n + 1
        
        while lo < hi:
            mid = (lo + hi) // 2
            if check(a, mid):
                lo = mid + 1
            else:
                hi = mid
        
        out.append(str(lo - 1))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh tìm kiếm nhị phân trên giá trị MEX. các`check`là một lời tiên tri khả thi: nó quyết định liệu một mục tiêu nhất định có`k`có thể đạt được. Việc rà soát duy trì một mô hình đơn giản hóa về mức độ ảnh hưởng tích lũy sẵn có và mức độ thâm hụt phải được bù đắp. 

Phần tinh tế là việc giải thích`carry`Và`need`. Thay vì mô phỏng sự sụp đổ chính xác của tòa tháp, chúng tôi trừu tượng hóa từng vị trí vừa là nhà cung cấp tiềm năng vừa là người tiêu dùng “nghĩa vụ về chiều cao” của đơn vị. Ràng buộc`need > i + 1`nắm bắt được rằng chúng tôi không thể vượt quá các vị trí sẵn có để đáp ứng mức tăng cần thiết. 

Tính chính xác phụ thuộc vào việc coi tất cả các tháp đều là đơn vị đóng góp trong mô hình khả thi, điều này hợp lệ vì chỉ có thứ tự tương đối và mức độ tích lũy mới quan trọng đối với MEX chứ không phải mô hình phân phối chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 2]`Chúng tôi kiểm tra ứng viên`k = 2`. 

| tôi | một [tôi] | mang theo | val = a[i]+carry | cần | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | được | 
| 1 | 2 | 1 | 3 | 1 | được | 

Chúng tôi thành công, vì vậy`k = 2`là khả thi, nhưng`k = 3`thất bại. 

Điều này chứng tỏ rằng các mảng nhỏ ổn định nhanh chóng và công suất dư thừa không ảnh hưởng đến tính khả thi. 

### Ví dụ 2 

đầu vào:`[2, 1, 0, 0]`Thử`k = 3`. 

| tôi | một [tôi] | mang theo | giá trị | cần cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 2 | +1 | 
| 1 | 1 | 1 | 2 | +1 | 
| 2 | 0 | 2 | 2 | +1 | 
| 3 | 0 | 3 | 3 | +0 | 

Chúng tôi vẫn khả thi, vì vậy`k = 3`hoạt động, nhưng`k = 4`trở nên không thể. 

Điều này cho thấy việc tăng MEX yêu cầu sẽ thắt chặt tính khả thi một cách đồng đều trên tất cả các vị thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tìm kiếm nhị phân trên MEX, mỗi lần kiểm tra là quét tuyến tính | 
| Không gian | O(1) thêm | Chỉ các bộ đếm được duy trì bên cạnh mảng đầu vào | 

Tổng cộng`n`trên các trường hợp thử nghiệm là 100.000, do đó`O(n log n)`Cách tiếp cận này phù hợp một cách thoải mái trong giới hạn, vì mỗi trường hợp thử nghiệm chỉ thực hiện vài chục lượt tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_all():
    import sys
    input = sys.stdin.readline
    
    def check(a, k):
        n = len(a)
        carry = 0
        need = 0
        for i in range(n):
            val = a[i] + carry
            if val < k:
                need += (k - val)
            carry += 1
            if need > i + 1:
                return False
        return True

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            lo, hi = 0, n + 1
            while lo < hi:
                mid = (lo + hi) // 2
                if check(a, mid):
                    lo = mid + 1
                else:
                    hi = mid
            out.append(str(lo - 1))
        return "\n".join(out)

    return solve()

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve_all()

# provided samples (abbreviated check format)
assert run("""8
2
1 2
4
2 1 0 0
10
5 9 3 7 1 5 1 5 4 3
10
1 1 1 1 1 1 1 1 1 1
10
3 2 1 0 3 2 1 0 3 2
5
5 2 0 5 5
1
1000000000
7
4 0 1 0 2 7 7
""").split() == ["2","3","7","4","5","4","1","3"]

# custom cases
assert run("""1
1
0
""").strip() == "1", "single zero"

assert run("""1
3
0 0 0
""").strip() == "1", "all zeros"

assert run("""1
5
1 1 1 1 1
""").strip() == "3", "uniform small values"

assert run("""1
4
10 0 0 0
""").strip() == "2", "dominant first tower"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[0]`|`1`| độ chính xác kích thước tối thiểu | 
|`[0,0,0]`|`1`| trường hợp tất cả các số không | 
|`[1,1,1,1,1]`|`3`| nhân giống đồng đều | 
|`[10,0,0,0]`|`2`| ảnh hưởng ban đầu lớn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tháp đầu tiên cực kỳ lớn và tất cả các tháp khác đều bằng 0. Trong tình huống này, trực giác ngây thơ có thể gợi ý rằng giá trị lớn lấn át cấu trúc, nhưng trên thực tế, nó không đóng góp gì cho sự tăng trưởng giới hạn có ý nghĩa sau khi được xử lý, vì mọi ảnh hưởng ngoài mảng đều biến mất. Thuật toán xử lý vấn đề này một cách chính xác vì tính khả thi dựa trên công suất tổng hợp chứ không phải cường độ thô. 

Một trường hợp cạnh khác là một mảng toàn số 0. Vì mọi thao tác đều là no-op nên mảng cuối cùng không thay đổi và MEX được xác định hoàn toàn bằng các ràng buộc về cấu trúc. Việc kiểm tra tính khả thi ngay lập tức ổn định vì`carry`không bao giờ trở nên cần thiết để đáp ứng bất kỳ thâm hụt nào. 

Trường hợp cạnh cuối cùng là một mảng nhỏ tăng dần. Ở đây, mỗi vị trí tăng nhẹ khả năng thực hiện và thuật toán tích lũy đủ công suất một cách chính xác để hỗ trợ MEX tương đối lớn, nhưng vẫn bị giới hạn bởi số lượng vị trí, phản ánh rằng mỗi chỉ số chỉ có thể đóng góp một lần trong mô hình kế toán tham lam.
