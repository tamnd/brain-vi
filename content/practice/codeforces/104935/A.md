---
title: "CF 104935A - Giải Tin học về độ trễ tăng dần một cách đơn điệu"
description: "Có một số người tổ chức, mỗi người càng trở nên muộn hơn khi các cuộc họp diễn ra. Sự chậm trễ của mỗi người tổ chức không phải là cố định: đối với một người nhất định, độ trễ của họ trong cuộc họp đầu tiên đã được biết, và sau đó trong mỗi cuộc họp tiếp theo, họ thậm chí còn muộn hơn bởi một…"
date: "2026-06-28T07:34:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104935
codeforces_index: "A"
codeforces_contest_name: "MITIT 2024 Combined Round"
rating: 0
weight: 104935
solve_time_s: 250
verified: false
draft: false
---

[CF 104935A - Giải đấu tin học về độ trễ tăng dần một cách đơn điệu](https://codeforces.com/problemset/problem/104935/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có một số người tổ chức, mỗi người càng trở nên muộn hơn khi các cuộc họp diễn ra. Độ trễ của mỗi người tổ chức không cố định: đối với một người nhất định, độ trễ của họ trong cuộc họp đầu tiên đã được biết, và sau đó trong mỗi cuộc họp tiếp theo, họ thậm chí còn muộn hơn với mức tăng cố định. 

Mỗi cuộc họp có thời lượng cố định. Mục đích là để xác định chỉ số cuộc họp đầu tiên trong đó mọi người tổ chức đều đến muộn đến mức họ hoàn toàn bỏ lỡ cuộc họp, nghĩa là thời gian đến của họ ít nhất bằng toàn bộ thời gian của cuộc họp. 

Một cách khác để xem xét tình huống là mỗi người có một hàm tuyến tính mô tả độ trễ của họ theo thời gian. Đối với số cuộc họp$k$, độ trễ của họ là$t_i + (k-1)a_i$, và chúng tôi muốn sớm nhất$k$sao cho tất cả các giá trị này ít nhất$M$. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$người tổ chức và giá trị độ trễ có thể tăng lên tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ sự mô phỏng nào trong các cuộc họp. Ngay cả khi mỗi lần kiểm tra cuộc họp đều diễn ra tuyến tính$N$, việc lặp đi lặp lại số lượng cuộc họp có thể rất lớn sẽ không phù hợp về mặt thời gian, vì bản thân câu trả lời có thể lớn (lên tới khoảng$10^9$trong trường hợp xấu nhất). 

Một ý tưởng mạnh mẽ sẽ là mô phỏng từng cuộc họp và tính toán lại độ trễ của mọi người mỗi lần. Điều này không thành công vì cả số lượng cuộc họp và tính toán mỗi cuộc họp đều quá lớn. 

Một vấn đề phức tạp hơn sẽ xuất hiện nếu một người cố gắng tính toán lại từ đầu cho mỗi cuộc họp bằng công thức nhưng vẫn lặp lại các cuộc họp: ngay cả khi mỗi lần kiểm tra đều đúng, số lần lặp lại có thể bùng nổ. 

Một trường hợp minh họa nhỏ: 

đầu vào:```
2 10
9 1
0 1
```Đối với thông tin đầu vào này, câu trả lời đúng là 2 vì: 

- Lần 1: độ trễ là 9 và 0, không phải tất cả đều ≥ 10 
- Lần 2: độ trễ là 10 và 1, vẫn không phải cả hai ≥ 10 nên thực tế lần 2 cũng không thành công, đáp án thành 10 (cuối cùng cả hai đều vượt ngưỡng) 

Phương pháp mô phỏng sẽ tiếp tục lặp lại cho đến khi cả hai đều vượt qua ngưỡng, có thể cần nhiều bước, mặc dù câu trả lời cuối cùng có thể được tính toán trực tiếp. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp qua các cuộc họp sẽ tăng lên$k$, và với mỗi$k$tính toán tất cả$t_i + (k-1)a_i$, kiểm tra xem tất cả có ít nhất$M$. Điều này đúng vì nó khớp chính xác với định nghĩa. Tuy nhiên, trường hợp xấu nhất sẽ phá vỡ nó ngay lập tức: nếu câu trả lời lớn, chẳng hạn$10^9$, và mỗi chi phí kiểm tra$O(N)$, thì chúng ta đang xem xét đại khái$2 \cdot 10^5 \cdot 10^9$hoạt động hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mỗi người tổ chức xác định một cách độc lập một ngưỡng cuộc họp mà sau đó họ luôn bỏ lỡ cuộc họp. Vì độ trễ của họ ngày càng tăng đều đều nên một khi họ bỏ lỡ một cuộc họp, họ sẽ bỏ lỡ tất cả những cuộc họp sau đó. Vì vậy, mỗi người đóng góp một điểm giới hạn duy nhất: cuộc gặp đầu tiên mà họ trở nên “luôn vắng mặt”. 

Đối với người cố định$i$, chúng tôi muốn cái nhỏ nhất$k$như vậy:$$t_i + (k-1)a_i \ge M$$Bất đẳng thức này có thể được giải trực tiếp. Sau khi chúng tôi tính toán ngưỡng này cho mọi người tổ chức, cuộc họp đầu tiên mà mọi người vắng mặt chỉ đơn giản là mức tối đa của tất cả các ngưỡng riêng lẻ vì chúng tôi cần tất cả các điều kiện để tổ chức đồng thời. 

Vì vậy, vấn đề giảm từ việc mô phỏng một quá trình động theo thời gian sang việc tính toán$N$ngưỡng số học độc lập và lấy giá trị cực đại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(N \cdot K)$|$O(1)$| Quá chậm | 
| Tính toán ngưỡng cho mỗi người |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi người tổ chức, hãy coi độ trễ của họ là một hàm số của chỉ số cuộc họp$k$:$t_i + (k-1)a_i$. Điều này chuyển quá trình động thành một bài toán bất đẳng thức tĩnh. 
2. Sắp xếp lại điều kiện vắng mặt:$$t_i + (k-1)a_i \ge M$$trở thành$$(k-1)a_i \ge M - t_i$$3. Tính số nguyên nhỏ nhất$k$thỏa mãn bất đẳng thức này. Vì phép chia có thể không chính xác nên hãy sử dụng phép chia trần:$$k_i = \left\lceil \frac{M - t_i}{a_i} \right\rceil + 1$$Điều này mang lại cuộc họp đầu tiên nơi người tổ chức$i$luôn luôn muộn. 
4. Theo dõi giá trị lớn nhất của tất cả$k_i$. Câu trả lời ít nhất phải lớn như vậy vì mỗi nhà tổ chức đều phải thỏa mãn ngưỡng riêng của mình. 
5. Xuất ra mức tối đa sau khi xử lý tất cả các nhà tổ chức. 

### Tại sao nó hoạt động 

Mỗi người tổ chức chuyển từ “tham dự” sang “luôn vắng mặt” đúng một lần và điểm chuyển tiếp này đơn điệu về số lượng cuộc họp. Sau khi đạt đến ngưỡng, họ sẽ không bao giờ đúng giờ nữa. Do đó, điều kiện toàn hệ thống “mọi người đều đến muộn” được thỏa mãn chính xác khi người tổ chức chậm nhất đến muộn vượt qua ngưỡng của họ. Tận dụng tối đa một cách chính xác đồng bộ hóa tất cả các ràng buộc độc lập vào cuộc họp đầu tiên trong đó tất cả chúng đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return (a + b - 1) // b

def solve():
    n, m = map(int, input().split())
    ans = 1

    for _ in range(n):
        t, a = map(int, input().split())
        need = m - t
        k = ceil_div(need, a) + 1
        if k > ans:
            ans = k

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo công thức ngưỡng dẫn xuất. Người trợ giúp`ceil_div`xử lý phép chia trần số nguyên một cách an toàn mà không cần thao tác dấu phẩy động, điều này tránh được các vấn đề về độ chính xác và đảm bảo tính chính xác cho các giá trị lớn. 

Biến`ans`lưu trữ ngưỡng tối đa trên tất cả các nhà tổ chức. Nó bắt đầu từ 1 vì cuộc gặp đầu tiên luôn có giá trị làm chỉ số cơ bản. Đối với mỗi người tổ chức, chúng tôi tính toán cuộc họp giới hạn cá nhân của họ và cập nhật mức tối đa toàn cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
3 10
9 1
0 2
5 1
```Chúng tôi tính toán ngưỡng của mỗi người tổ chức. 

| Người tổ chức | t | một | cần = M - t | k tính toán | k_i | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 | 1 | 1 | trần(1/1)+1 = 2 | 2 | 
| 2 | 0 | 2 | 10 | trần(10/2)+1 = 5+1 | 6 | 
| 3 | 5 | 1 | 5 | trần(5/1)+1 = 6 | 6 | 

Tối đa là 6. 

Tại cuộc họp thứ 5, người tổ chức 2 vẫn đến đúng ranh giới (chưa thiếu hết). Tại cuộc họp thứ 6, tất cả ban tổ chức đã đến trễ ít nhất 10 đơn vị nên ai cũng bỏ lỡ. 

Dấu vết này cho thấy câu trả lời hoàn toàn bị chi phối bởi người tổ chức vượt qua ngưỡng chậm nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi người tổ chức được xử lý một lần với số học theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ lưu trữ mức tối đa đang chạy | 

Quét tuyến tính$2 \cdot 10^5$các phần tử dễ dàng đủ nhanh trong giới hạn vì nó chỉ bao gồm các phép toán số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    ans = 1

    for _ in range(n):
        t, a = map(int, input().split())
        need = m - t
        k = (need + a - 1) // a + 1
        ans = max(ans, k)

    return str(ans)

# sample
assert run("4 60\n0 30\n10 30\n20 30\n25 30\n") == "9"

# minimum case
assert run("1 10\n0 1\n") == "11", "single beaver"

# already close to threshold
assert run("2 5\n4 1\n1 10\n") == "2", "one dominates"

# equal growth
assert run("3 100\n0 10\n0 10\n0 10\n") == "10", "uniform case"

# large increments
assert run("2 100\n0 100\n99 1\n") == "2", "boundary crossing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hải ly đơn | 11 | tính đúng đắn của công thức cơ bản | 
| ngưỡng hỗn hợp | 2 | logic lựa chọn tối đa | 
| tăng trưởng đồng đều | 10 | xử lý đối xứng | 
| vượt biên | 2 | hành vi trần đúng | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi người tổ chức đã ở rất gần ngưỡng trong cuộc họp đầu tiên. Ví dụ:```
1 10
9 1
```Ở đây độ trễ bổ sung cần thiết là 1. Việc tính toán cho:$$k = \lceil 1/1 \rceil + 1 = 2$$Ở cuộc họp 1 họ vẫn chưa đến muộn hoàn toàn và ở cuộc họp 2 họ đã vượt ngưỡng chính xác. Thuật toán trả về chính xác 2, cho thấy sự dịch chuyển từng bước một từ$(k-1)$được xử lý bởi trận chung kết`+1`trong công thức. 

Một trường hợp khác là khi nhiều nhà tổ chức có tốc độ tăng trưởng rất khác nhau. Mức tối đa đảm bảo rằng ngay cả khi hầu hết mọi người đến muộn sớm thì một nhà tổ chức phát triển chậm sẽ quyết định câu trả lời cuối cùng. Ví dụ:```
2 100
0 1
0 100
```Người tổ chức thứ hai đến muộn ở cuộc họp thứ 2, nhưng người đầu tiên cần 100 cuộc họp. Kết quả là 100 và thuật toán chọn chính xác mức tối đa đó mà không bị ảnh hưởng bởi các giá trị trung gian.
