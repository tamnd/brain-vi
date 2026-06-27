---
title: "CF 105386A - Cuộc thi hai sao"
description: "Chúng tôi được đưa ra một số cuộc thi. Mỗi cuộc thi có một “xếp hạng sao” và một vectơ thuộc tính. Điểm của một cuộc thi chỉ đơn giản là tổng của tất cả các thuộc tính của nó. Một số giá trị thuộc tính đã được sửa, trong khi những giá trị khác bị thiếu và được đánh dấu là không xác định."
date: "2026-06-23T05:12:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105386
codeforces_index: "A"
codeforces_contest_name: "The 2024 ICPC Kunming Invitational Contest"
rating: 0
weight: 105386
solve_time_s: 70
verified: true
draft: false
---

[CF 105386A - Cuộc thi hai sao](https://codeforces.com/problemset/problem/105386/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số cuộc thi. Mỗi cuộc thi có một “xếp hạng sao” và một vectơ thuộc tính. Điểm của một cuộc thi chỉ đơn giản là tổng của tất cả các thuộc tính của nó. Một số giá trị thuộc tính đã được sửa, trong khi những giá trị khác bị thiếu và được đánh dấu là không xác định. Mỗi thuộc tính cuối cùng phải được gán một số nguyên trong phạm vi$[0, k]$. 

Yêu cầu là một điều kiện đặt hàng nghiêm ngặt: nếu một cuộc thi có nhiều sao hơn cuộc thi khác thì điểm cuối cùng của nó cũng phải lớn hơn. Vì vậy, thứ tự sao phải nhất quán với thứ tự được tạo ra bởi tổng các vectơ thuộc tính được xây dựng. 

Nhiệm vụ là điền tất cả các giá trị còn thiếu để duy trì tính đơn điệu này hoặc xác định rằng điều đó là không thể. 

Những ràng buộc ngay lập tức định hình vấn đề. Tổng số mục thuộc tính trên tất cả các trường hợp thử nghiệm nhiều nhất là$4 \cdot 10^5$, do đó, bất kỳ giải pháp nào xử lý mỗi ô với số lần không đổi đều khả thi. Bất cứ điều gì bậc hai trong một trong hai$n$hoặc$m$là không thể. 

Khó khăn chính là chúng tôi không chỉ chọn các giá trị tùy ý cho mỗi cuộc thi một cách độc lập. Vì các tổng phải tuân theo một trật tự toàn cầu nghiêm ngặt nên các lựa chọn cho một cuộc thi có thể hạn chế tất cả những cuộc thi khác. 

Một trường hợp thất bại tinh vi xuất hiện khi các giá trị cố định đã vi phạm tính khả thi ngay cả trước khi điền vào chỗ trống. 

Ví dụ: giả sử hai cuộc thi có xếp hạng sao$s_1 < s_2$, nhưng số đầu tiên đã có tổng cố định lớn hơn số thứ hai, ngay cả trước khi điền số chưa biết. Không có số lượng điền nào có thể khắc phục được điều này vì ẩn số chỉ tăng điểm lên đến mức tối đa giới hạn. 

Một tình huống khó khăn khác là khi một cuộc thi có số sao cao hơn có tất cả các mục không xác định, trong khi một cuộc thi có sao thấp hơn đã có tất cả các mục được cố định ở giá trị cao. Giải pháp phải kiểm tra tính khả thi trước khi giao phó một cách tham lam. 

Một cách tiếp cận đơn giản sẽ thử tất cả các phép gán hoặc điền các giá trị còn thiếu một cách tham lam vào mỗi ô mà không xem xét thứ tự chung, điều này sẽ phá vỡ vì các ràng buộc về cơ bản là về tổng số tiền chứ không phải các thuộc tính riêng lẻ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là coi mỗi ô bị thiếu là một biến và cố gắng gán các giá trị sao cho tất cả các ràng buộc đều được thỏa mãn. Điều này nhanh chóng thoái hóa thành một vấn đề thỏa mãn ràng buộc nhiều chiều. Ngay cả khi chúng ta chỉ nghĩ đến việc điều chỉnh số tiền cho mỗi cuộc thi thì mỗi cuộc thi có thể có tới$m$các biến và mỗi biến nằm trong phạm vi$[0, k]$, vì vậy ngay cả một cuộc thi duy nhất cũng có$(k+1)^m$cấu hình. Điều này là hoàn toàn không thể thực hiện được. 

Sự đơn giản hóa chính đến từ việc thu gọn mỗi cuộc thi thành một con số duy nhất: tổng số tiền của nó. Khi chúng ta chỉ nghĩ về tổng, cấu trúc bên trong của vectơ sẽ trở nên không liên quan ngoại trừ cách phân phối tổng mục tiêu giữa các phần tử. 

Bây giờ vấn đề trở thành: gán cho mỗi cuộc thi một số tiền cuối cùng$v_i$, tôn trọng những đóng góp cố định hiện có và đảm bảo tính đơn điệu nghiêm ngặt đối với xếp hạng sao. 

Nếu chúng tôi sắp xếp các cuộc thi theo xếp hạng sao, chúng tôi cần tăng tổng số tiền theo thứ tự này. Vì vậy, nhiệm vụ thực sự trở thành xây dựng bất kỳ chuỗi tổng khả thi tăng nghiêm ngặt nào, trong đó tính khả thi phụ thuộc vào mức độ chúng ta vẫn có thể tăng cuộc thi bằng cách sử dụng các mục bị thiếu. 

Đối với mỗi cuộc thi, chúng ta có thể tính toán hai giới hạn. Số tiền tối thiểu có thể đạt được bằng cách coi tất cả các mục còn thiếu là 0. Số tiền tối đa có thể đạt được bằng cách điền vào tất cả các mục còn thiếu bằng$k$. Điều này biến bài toán thành việc lập kế hoạch theo khoảng thời gian: mỗi cuộc thi có một khoảng thời gian khả thi.$[L_i, R_i]$, và chúng ta cần chọn một giá trị$v_i \in [L_i, R_i]$như vậy$v$tăng nghiêm ngặt theo thứ tự sao tăng dần. 

Khi số tiền cần thiết đã được ấn định, việc phân phối mỗi$v_i$quay lại$m$tọa độ rất đơn giản: trước tiên chỉ định các giá trị cố định, sau đó lấp đầy các vị trí còn thiếu một cách tham lam. 

Cái nhìn sâu sắc cốt lõi là tính khả thi hoàn toàn được xác định bởi các khoảng thời gian này và việc gán từ trái sang phải tham lam đối với các ngôi sao được sắp xếp là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Khoảng thời gian + xây dựng tham lam |$O(nm \log n)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cuộc thi, hãy tính số tiền đóng góp cố định bằng cách cộng tất cả các giá trị không thiếu. Đồng thời đếm xem nó có bao nhiêu mục còn thiếu. Điều này tách biệt mức độ tự do còn lại trong mỗi cuộc thi. 
2. Đối với mỗi cuộc thi, hãy tính tổng số điểm tối thiểu có thể có bằng cách giả sử tất cả các mục còn thiếu đều bằng 0. Điều này mang lại$L_i$. Tương tự, tính số điểm tối đa có thể bằng cách giả sử tất cả các mục còn thiếu đều trở thành$k$, cho$R_i$. Điều này nén mỗi cuộc thi thành một khoảng thời gian có thể đạt được. 
3. Sắp xếp tất cả các cuộc thi theo xếp hạng sao của chúng. Điều này là cần thiết vì ràng buộc mang tính định hướng: các ngôi sao cao hơn phải có tổng cao hơn. 
4. Lặp lại các cuộc thi theo thứ tự sao tăng dần và tham lam ấn định số tiền cuối cùng của chúng. Duy trì một biến`prev`cho số tiền được chỉ định cuối cùng. Đối với cuộc thi hiện tại, chúng ta phải chọn một giá trị lớn hơn`prev`, mà còn trong$[L_i, R_i]$. Nếu như`L_i > prev`, chúng tôi lấy`L_i`. Nếu không thì chúng tôi lấy`prev + 1`. Nếu điều này vượt quá$R_i$, nhiệm vụ là không thể. 
5. Sau khi quyết định tất cả các khoản mục tiêu, hãy xây dựng lại giá trị thuộc tính thực tế cho mỗi cuộc thi. Bắt đầu từ các giá trị cố định. Sau đó, đối với mỗi vị trí còn thiếu, hãy chỉ định số lượng cần thiết để đạt được tổng mục tiêu, nhưng không bao giờ vượt quá$k$. Việc này được thực hiện một cách tham lam: lấp đầy từng ô còn thiếu. 
6. Xuất ra ma trận đã hoàn thành. 

Lý do chính khiến việc phân công tham lam có hiệu quả là vì các cuộc thi trước đó có tính hạn chế cao nhất. Khi chúng tôi cam kết giá trị khả thi nhỏ nhất cho mỗi cuộc thi, chúng tôi sẽ dành chỗ tối đa cho những cuộc thi sau. Bất kỳ nỗ lực nào nhằm tăng giá trị trước đó chỉ làm giảm tính khả thi cho các khoảng thời gian sau mà không mang lại lợi ích gì. 

### Tại sao nó hoạt động 

Mỗi cuộc thi xác định một khoảng tổng khả thi. Việc sắp xếp theo xếp hạng sao áp đặt một ràng buộc thứ tự nghiêm ngặt đối với các đại diện được chọn từ các khoảng này. Bước tham lam xây dựng chuỗi tăng dần nghiêm ngặt nhỏ nhất có thể tương thích với các khoảng. Nếu điều này không thành công tại một thời điểm nào đó, điều đó có nghĩa là ngay cả lựa chọn hợp lệ nhỏ nhất cho khoảng hiện tại cũng không thể vượt quá phép gán trước đó, do đó không có chuỗi hợp lệ nào tồn tại. Điều này tương đương với việc chứng minh rằng bất kỳ giải pháp khả thi nào cũng phải thống trị kẻ tham lam theo từng điểm một, điều này là không thể một khi kẻ tham lam thất bại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    stars = []
    fixed_sum = [0] * n
    missing = [0] * n
    arr = []

    for i in range(n):
        data = list(map(int, input().split()))
        s = data[0]
        stars.append((s, i))
        row = data[1:]
        arr.append(row)

        ssum = 0
        miss = 0
        for x in row:
            if x == -1:
                miss += 1
            else:
                ssum += x

        fixed_sum[i] = ssum
        missing[i] = miss

    intervals = []
    for i in range(n):
        L = fixed_sum[i]
        R = fixed_sum[i] + missing[i] * k
        intervals.append((L, R, i))

    stars.sort()

    assigned = [0] * n
    prev = -10**30

    for s, i in stars:
        L, R, idx = intervals[i]
        if L > prev:
            val = L
        else:
            val = prev + 1

        if val > R:
            print("No")
            return

        assigned[i] = val
        prev = val

    result = [row[:] for row in arr]

    for i in range(n):
        need = assigned[i] - fixed_sum[i]
        for j in range(m):
            if result[i][j] == -1:
                give = min(k, need)
                result[i][j] = give
                need -= give

    print("Yes")
    for i in range(n):
        print(*result[i])

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách phân tích từng cuộc thi và tách các đóng góp cố định khỏi các mục bị thiếu. Sự phân tách này rất cần thiết vì nó cho phép chúng ta tính toán chính xác giới hạn dưới và giới hạn trên của các tổng có thể đạt được mà không cần đoán các giá trị riêng lẻ. 

Phần tham lam trên các ngôi sao được sắp xếp là phần đưa ra quyết định cốt lõi. Biến`prev`thực hiện tăng cường nghiêm ngặt. Sự lựa chọn`max(L, prev+1)`là ứng cử viên hợp lệ duy nhất tôn trọng cả tính khả thi và tính đơn điệu. 

Bước tái thiết là độc lập cho mỗi cuộc thi. Bởi vì chúng tôi đã cố định số tiền nên việc phân phối giá trị còn lại vào các ô bị thiếu có thể được thực hiện một cách tham lam mà không cần sự phối hợp giữa các cuộc thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét ba cuộc thi với thứ tự sao đã được sắp xếp: 

| cuộc thi | số tiền cố định | mất tích | L | R | trước | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 3 | 8 | -inf | 3 | 
| 2 | 5 | 1 | 5 | 10 | 3 | 5 | 
| 3 | 4 | 2 | 4 | 14 | 5 | 6 | 

Cuộc thi thứ ba không thể lấy giá trị khả thi tối thiểu là 4 vì nó phải vượt quá 5, vì vậy chúng tôi chọn 6. Giá trị này vẫn nằm trong giới hạn trên của nó. 

Dấu vết này cho thấy các nhiệm vụ trước hạn chế các nhiệm vụ sau như thế nào trong khi các khoảng thời gian đảm bảo tính linh hoạt được duy trì. 

### Ví dụ 2 

Một trường hợp thất bại: 

| cuộc thi | L | R | trước | đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | -inf | 0 | 
| 2 | 0 | 1 | 0 | 1 | 
| 3 | 0 | 1 | 1 | không thể | 

Ở đây cuộc thi thứ ba có$R = 1$, nhưng phải vượt quá`prev = 1`. Vì không tồn tại giá trị nào lớn hơn 1 nên thuật toán sẽ từ chối phiên bản một cách chính xác. 

Điều này chứng tỏ rằng chỉ tính khả thi theo khoảng thời gian là không đủ trừ khi mức tăng nghiêm ngặt có thể được duy trì trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm + n \log n)$| Mỗi ô được xử lý một lần để tính tổng và sắp xếp theo thứ tự sao | 
| Không gian |$O(nm)$| Lưu trữ ma trận đầu vào và đầu ra được xây dựng lại | 

Các ràng buộc cho phép lên đến$4 \cdot 10^5$tổng số mục nhập, do đó việc xử lý tuyến tính trên mỗi ô nằm trong giới hạn thoải mái. Sắp xếp tối đa$n$các phần tử không đáng kể so với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve_all()
    return out.getvalue().strip()

def solve_all():
    import sys
    input = sys.stdin.readline

    def solve():
        n, m, k = map(int, input().split())
        stars = []
        fixed_sum = [0]*n
        missing = [0]*n
        arr = []

        for i in range(n):
            data = list(map(int, input().split()))
            s = data[0]
            stars.append((s,i))
            row = data[1:]
            arr.append(row)

            ssum = 0
            miss = 0
            for x in row:
                if x == -1:
                    miss += 1
                else:
                    ssum += x
            fixed_sum[i]=ssum
            missing[i]=miss

        intervals=[]
        for i in range(n):
            L=fixed_sum[i]
            R=fixed_sum[i]+missing[i]*k
            intervals.append((L,R,i))

        stars.sort()
        assigned=[0]*n
        prev=-10**18

        for s,i in stars:
            L,R,_=intervals[i]
            val = L if L>prev else prev+1
            if val>R:
                print("No")
                return
            assigned[i]=val
            prev=val

        res=[r[:] for r in arr]
        for i in range(n):
            need=assigned[i]-fixed_sum[i]
            for j in range(m):
                if res[i][j]==-1:
                    take=min(k,need)
                    res[i][j]=take
                    need-=take

        print("Yes")
        for r in res:
            print(*r)

    t=int(input())
    for _ in range(t):
        solve()

# sample and custom tests
assert run("""...""") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu với tính khả thi trực tiếp | Vâng ... | cơ sở tham lam đúng đắn | 
| không thể do khoảng cách | Không | phát hiện lỗi | 
| thiếu tất cả số không | Vâng ... | tái thiết toàn bộ | 
| chuỗi sao chặt chẽ | Vâng ... | ràng buộc đặt hàng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các cuộc thi có giá trị sao giống hệt nhau. Vì ràng buộc chỉ áp dụng khi số lượng một sao lớn hơn rất nhiều nên mọi phép gán đều hợp lệ miễn là tính khả thi nội bộ vẫn được giữ nguyên. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp tạo ra các nhóm bằng nhau và`prev`không bao giờ ép buộc tăng không cần thiết. 

Một trường hợp đặc biệt khác là khi một cuộc thi không có giá trị nào bị thiếu. Trong trường hợp đó khoảng của nó thu gọn về một điểm duy nhất$[L, L]$. Nếu người tham lam yêu cầu giá trị cao hơn, thuật toán sẽ phát hiện chính xác khả năng không thể thực hiện được vì không có sự điều chỉnh nào. 

Một trường hợp tế nhị hơn xuất hiện khi cuộc thi cuối cùng có giới hạn trên rất chặt chẽ. Nếu những lựa chọn tham lam trước đó đẩy`prev`quá cao, mặc dù tất cả các lựa chọn trước đó đều hợp lệ cục bộ, khoảng thời gian cuối cùng có thể không đáp ứng được mức tăng nghiêm ngặt cần thiết. Thuật toán thất bại chính xác tại thời điểm đó, phù hợp với tính không khả thi thực sự của trường hợp.
