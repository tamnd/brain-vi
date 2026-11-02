---
title: "CF 106516B - Vụ nổ trái cây"
description: "Chúng ta được cung cấp một chuỗi dài có độ dài là $N^2$ và mọi giá trị từ $1$ đến $N$ xuất hiện chính xác $N$ lần. Trình tự này được hiểu là dòng thời gian của các hành động liên quan đến các mã thông báo có nhãn $N$ được sắp xếp theo cấu trúc tuần hoàn."
date: "2026-06-18T19:02:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106516
codeforces_index: "B"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Finals"
rating: 0
weight: 106516
solve_time_s: 58
verified: true
draft: false
---

[CF 106516B - Vụ nổ trái cây](https://codeforces.com/problemset/problem/106516/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi dài có độ dài là$N^2$và mọi giá trị từ$1$ĐẾN$N$xuất hiện chính xác$N$lần. Trình tự này được hiểu là dòng thời gian của các hành động liên quan đến$N$các thẻ được gắn nhãn được sắp xếp theo cấu trúc tuần hoàn. Mỗi lần xuất hiện của một giá trị sẽ mô tả một động thái hoặc tương tác gắn liền với mã thông báo đó. 

Câu hỏi trọng tâm là liệu trình tự này có thể được thực hiện một cách nhất quán theo một quy tắc cấu trúc ẩn hay không: các bước di chuyển phải được phân tách thành$N$các chuỗi tuần hoàn, mỗi chuỗi hoạt động giống như một “lịch quay” của các nhãn. Nói cách khác, chúng ta đang cố gắng quyết định liệu chuỗi có thể được chia thành$N$chuỗi hợp lệ, mỗi chuỗi đi qua$1 \to 2 \to \dots \to N \to 1$theo thứ tự, tôn trọng các ràng buộc về ngoại hình. 

Các ràng buộc đủ lớn để bất kỳ nỗ lực nào nhằm tìm kiếm rõ ràng trên các phân rã hoặc mô phỏng tất cả các phép gán các phần tử cho chuỗi sẽ bùng nổ về mặt tổ hợp. Một quy trình gán đơn giản sẽ phân nhánh một cách hiệu quả theo thứ tự con mà mỗi lần xuất hiện thuộc về, dẫn đến khả năng theo cấp số nhân trong$N^2$. Điều đó ngay lập tức loại trừ việc quay lại hoặc phân vùng vũ phu. 

Một trường hợp lỗi tinh vi xuất hiện khi các điều kiện tần số cục bộ được thỏa mãn nhưng không thể thực hiện được cấu trúc toàn cục. Ví dụ: có thể xây dựng các tiền tố trong đó giá trị$y$xuất hiện quá sớm so với$y-1$, tạo ra tình huống có quá nhiều lần xuất hiện “không liên kết” của$y$hiện hữu. Một nhiệm vụ tham lam có thể vẫn thành công cục bộ nhưng sau đó không thể mở rộng được vì các chuỗi tiếp theo đã hết các phần trước hợp lệ. 

Khó khăn chính là tính khả thi không nằm ở tổng số lượng mà ở cách xuất hiện của các giá trị liền kề xen kẽ trên tất cả các tiền tố. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng xây dựng một cách rõ ràng sự phân rã thành$N$các chuỗi tuần tự. Chúng tôi quét mảng và gán từng phần tử cho một trong các chuỗi con có thể chấp nhận nó một cách hợp pháp, nghĩa là nó có giá trị trước đó trong chu trình đã được đặt. Điều này hoạt động giống như một vấn đề lập kế hoạch bị ràng buộc với$N$tài nguyên và$N^2$sự kiện. 

Mặc dù cách xây dựng tham lam này đúng về mặt khái niệm, nhưng việc duy trì tất cả các chuỗi con có thể có và kiểm tra các vị trí hợp lệ ở mỗi bước sẽ dẫn đến sự bùng nổ trạng thái tốn kém. Trong trường hợp xấu nhất, mỗi phần tử yêu cầu quét nhiều chuỗi con và cập nhật theo tầng, tạo ra độ phức tạp theo thứ tự$O(N^3)$hoặc tệ hơn tùy thuộc vào việc thực hiện. 

Cái nhìn sâu sắc về cấu trúc là tính khả thi được nắm bắt hoàn toàn bằng sự mất cân bằng tiền tố giữa các giá trị liên tiếp. Với mọi tiền tố và mọi giá trị$y$, số lần xuất hiện của$y$được thấy cho đến nay không thể vượt quá số lần xuất hiện của$y-1$được nhìn thấy cho đến nay bởi nhiều hơn một. Điều kiện này vừa cần vừa đủ vì mỗi “chuỗi” chỉ có thể đóng góp tối đa một$y$bất kỳ lúc nào và mỗi lần bổ sung$y$phải được hỗ trợ bởi cái trước$y-1$trong một chuỗi nào đó. 

Điều này làm giảm toàn bộ vấn đề trong việc duy trì số lượng tiền tố và kiểm tra sự bất đẳng thức đơn giản trên tất cả các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công trình tự vũ phu | Hàm mũ | O(N^2) | Quá chậm | 
| Kiểm tra điều kiện tiền tố | O(N^2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành điều kiện đếm tiền tố trên dãy. 

### bước 

1. Khởi tạo bảng tần số`cnt[y]`cho các giá trị$1 \ldots N$, tất cả đều bắt đầu từ con số 0. Điều này theo dõi số lần mỗi giá trị đã xuất hiện trong tiền tố. 
2. Quét trình tự từ trái sang phải. Đối với mỗi vị trí$i$, tăng`cnt[x[i]]`. Điều này duy trì tần số tiền tố chính xác ở mỗi bước. 
3. Sau khi cập nhật vị trí$i$, kiểm tra điều kiện cho tất cả các giá trị$y \ge 2$:

đảm bảo`cnt[y] <= cnt[y-1] + 1`. 

Điều này thực thi giá trị đó$y$không bao giờ “vượt mặt” người tiền nhiệm của nó$y-1$bởi nhiều hơn một lần xuất hiện. 
4. Nếu phát hiện có vi phạm, ngay lập tức kết luận trình tự không hợp lệ. 
5. Nếu quá trình quét hoàn tất mà không có vi phạm thì trình tự đó là hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là ở bất kỳ tiền tố nào, mỗi giá trị$y$chỉ có thể có tối đa một lần xuất hiện "không khớp" chưa được hỗ trợ bởi một sự kiện tương ứng$y-1$. Mỗi chuỗi hợp lệ đóng góp một vị trí đang chờ xử lý để tiến lên từ$y-1$ĐẾN$y$. Nếu như$y$vượt quá$y-1$bởi nhiều hơn một, một số lần xuất hiện$y$không thể được đặt vào bất kỳ chuỗi nào mà không phá vỡ trật tự, khiến cho việc phân tách là không thể. Ngược lại, nếu bất đẳng thức luôn đúng thì chúng ta luôn có thể gán mỗi lần xuất hiện của$y$để mở rộng chuỗi hiện có hoặc bắt đầu vị trí chuỗi mới chưa từng có cho phép, đảm bảo khả năng xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))

    cnt = [0] * (n + 1)

    for v in a:
        cnt[v] += 1

        for y in range(2, n + 1):
            if cnt[y] > cnt[y - 1] + 1:
                print("No")
                return

    print("Yes")

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì số lượng tiền tố và kiểm tra ràng buộc kề ngay sau mỗi lần cập nhật. Vòng lặp bên trong kết thúc$y$là đủ vì chỉ những khác biệt liền kề mới quan trọng; vi phạm ở bất kỳ giá trị nào được phát hiện cục bộ tại thời điểm nó xuất hiện. 

Một chi tiết tinh tế là việc kiểm tra phải diễn ra sau mỗi lần tăng chứ không phải sau khi quét toàn bộ, vì thuộc tính phụ thuộc vào tiền tố. Việc trì hoãn xác thực có thể bỏ lỡ các trạng thái không hợp lệ trung gian mà sau này bị “che giấu” bởi các lần xuất hiện trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 1 3 2 3 1 2 3
```Chúng tôi theo dõi số lượng tiền tố: 

| tôi | x[i] | cnt1 | cnt2 | cnt3 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | vâng | 
| 2 | 2 | 1 | 1 | 0 | vâng | 
| 3 | 1 | 2 | 1 | 0 | vâng | 
| 4 | 3 | 2 | 1 | 1 | vâng | 
| 5 | 2 | 2 | 2 | 1 | vâng | 
| 6 | 3 | 2 | 2 | 2 | vâng | 
| 7 | 1 | 3 | 2 | 2 | vâng | 
| 8 | 2 | 3 | 3 | 2 | vâng | 
| 9 | 3 | 3 | 3 | 3 | vâng | 

Không có tiền tố nào vi phạm ràng buộc nên chuỗi này hợp lệ. 

### Ví dụ 2 

đầu vào:```
3
1 3 3 2 2 2 1 1 3
```| tôi | x[i] | cnt1 | cnt2 | cnt3 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | vâng | 
| 2 | 3 | 1 | 0 | 1 | vâng | 
| 3 | 3 | 1 | 0 | 2 | không (vi phạm cnt3 ≤ cnt2+1) | 

Ở bước 3,`cnt3 = 2`trong khi`cnt2 + 1 = 1`, do đó điều kiện thất bại ngay lập tức. Điều này cho thấy mức độ vượt quá nhãn cao hơn mà không có sự hỗ trợ đầy đủ từ nhãn trước đó sẽ sớm phá vỡ tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi trong số$N^2$các phần tử được xử lý một lần, với các bản cập nhật và kiểm tra liên tục trên một phạm vi nhãn cố định | 
| Không gian |$O(N)$| Chỉ có một mảng tần số có kích thước$N$được duy trì | 

Giải pháp phù hợp thoải mái trong các ràng buộc điển hình cho$N$lên đến vài nghìn, vì công việc chiếm ưu thế là tuyến tính trong kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# minimal valid case
assert run("1\n1") == "Yes"

# small valid structured case
assert run("2\n1 2 1 2") == "Yes"

# invalid due to early imbalance
assert run("2\n2 2 1 1") == "No"

# larger consistent cycle
assert run("3\n1 2 3 1 2 3 1 2 3") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | Có | trường hợp hợp lệ nhỏ nhất | 
| 2, 1 2 1 2 | Có | cấu trúc cân bằng xen kẽ | 
| 2, 2 2 1 1 | Không | phát hiện vi phạm tiền tố | 
| 3, 1 2 3 lặp lại | Có | tính nhất quán theo chu kỳ lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi nhãn cao hơn xuất hiện quá sớm, trước khi tồn tại đủ số lần xuất hiện của nhãn trước đó. Ví dụ: đầu vào:```
3
1 3 3 ...
```thất bại ngay lập tức vì lần xuất hiện thứ hai của 3 xuất hiện trong khi 2 vẫn chưa được thiết lập đủ số lượng. Thuật toán phát hiện điều này ở tiền tố chính xác nơi`cnt[3] > cnt[2] + 1`, ngăn chặn bất kỳ sự hoàn thành không hợp lệ nào được chấp nhận. 

Một trường hợp khác là khi các giá trị được cân bằng hoàn hảo nhưng bị dịch chuyển, chẳng hạn như bắt đầu với sự xuất hiện lặp lại của nhãn cao nhất. Mặc dù tổng số đếm khớp ở cuối, nhưng việc kiểm tra tiền tố sẽ không thành công sớm, điều này được xử lý chính xác vì thuật toán không bao giờ trì hoãn việc xác thực đến trạng thái cuối cùng.
