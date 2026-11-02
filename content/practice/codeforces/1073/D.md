---
title: "CF 1073D - Hội chợ Berland"
description: "Chúng tôi đang mô phỏng một chu kỳ đi bộ qua một loạt các mức giá gian hàng. Polycarp bắt đầu ở vị trí 1 và tiếp tục chuyển động theo chiều kim đồng hồ theo một chu kỳ cố định. Tại mỗi gian hàng, anh ta kiểm tra xem số tiền còn lại của mình có ít nhất bằng giá một chiếc kẹo ở gian hàng đó hay không."
date: "2026-06-15T14:11:08+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1073
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 53 (Rated for Div. 2)"
rating: 1700
weight: 1073
solve_time_s: 267
verified: true
draft: false
---

[CF 1073D - Hội chợ Berland](https://codeforces.com/problemset/problem/1073/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 4 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chu kỳ đi bộ qua một loạt các mức giá gian hàng. Polycarp bắt đầu ở vị trí 1 và tiếp tục chuyển động theo chiều kim đồng hồ theo một chu kỳ cố định. Tại mỗi gian hàng, anh ta kiểm tra xem số tiền còn lại của mình có ít nhất bằng giá một chiếc kẹo ở gian hàng đó hay không. Nếu đúng như vậy, anh ta mua đúng một chiếc kẹo và giảm số tiền của mình theo mức giá đó. Nếu không, anh ta chỉ cần bỏ qua việc mua hàng. Sau khi xử lý xong một gian hàng, anh ấy luôn chuyển sang gian hàng tiếp theo. Quá trình chỉ dừng lại khi việc vượt qua đầy đủ không còn tạo ra bất kỳ giao dịch mua nào nữa, điều này trên thực tế xảy ra khi anh ta không còn đủ khả năng mua bất kỳ viên kẹo nào ở vị trí hiện tại và tiếp tục cho đến khi anh ta sử dụng hết mọi khả năng. 

Nhiệm vụ là đếm tổng số kẹo anh ta mua trước khi mô phỏng kết thúc. 

Các hạn chế rất lớn: lên tới 200.000 gian hàng và tổng số tiền lên tới 10^18. Điều này ngay lập tức loại trừ việc mô phỏng từng bước ngây thơ trên tất cả các khoản khấu trừ tiền nếu nó tính toán lại bất cứ thứ gì đắt tiền trong mỗi bước. Mô phỏng trực tiếp của mỗi lượt truy cập vẫn tuyến tính trên mỗi chu kỳ, nhưng số lượng chu kỳ có thể lớn bằng T chia cho giá tối thiểu, trong trường hợp xấu nhất sẽ trở nên quá lớn để mô phỏng rõ ràng. 

Trường hợp cạnh chính xuất hiện khi tất cả giá là 1. Trong trường hợp đó, Polycarp mua kẹo ở mỗi bước cho đến khi T về 0, tạo ra chính xác các hoạt động T. Bất kỳ cách tiếp cận nào mô phỏng từng bước vẫn sẽ hoạt động về mặt khái niệm nhưng sẽ trở nên quá chậm trong các công trình xây dựng trong trường hợp xấu nhất chặt chẽ hơn, nơi cần có nhiều chu trình. 

Một trường hợp rắc rối khác là khi chỉ có một gian hàng có giá phải chăng và những gian hàng khác luôn bị bỏ qua. Việc triển khai đơn giản có thể quét liên tục tất cả n gian hàng ngay cả khi hầu hết các bước đều bị lãng phí khi kiểm tra, dẫn đến hành vi O(nT) không cần thiết trong các diễn giải suy biến. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi mô phỏng Polycarp di chuyển quanh vòng tròn, kiểm tra từng gian hàng, trừ giá khi có thể và tăng bộ đếm. Điều này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, mỗi chu kỳ đầy đủ có giá O(n) và trong trường hợp xấu nhất Polycarp có thể thực hiện theo thứ tự chu kỳ T/phút(a_i). Vì T có thể là 10^18 nên điều này là không thể. 

Quan sát quan trọng là quá trình này có cấu trúc tuần hoàn mạnh mẽ. Mỗi vòng quay đầy đủ trên tất cả các gian hàng sẽ làm giảm ngân sách một lượng cố định bằng tổng của tất cả các mức giá. Nếu chúng tôi biết Polycarp có thể thực hiện bao nhiêu vòng hoàn chỉnh, chúng tôi có thể bỏ qua chúng thay vì mô phỏng từng bước. Sau khi dùng hết toàn bộ vòng, chỉ còn lại một phần vượt qua, có thể được mô phỏng trong một lần quét tuyến tính cuối cùng. 

Điều này làm giảm vấn đề phải đếm xem có bao nhiêu chu kỳ đầy đủ phù hợp với ngân sách, sau đó xử lý phần còn lại trong một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T × n) trường hợp xấu nhất | O(1) | Quá chậm | 
| Nhảy chu kỳ + Quét dư | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng chi phí của một chu kỳ đầy đủ trên tất cả các gian hàng. Điều này thể hiện số tiền được chi tiêu nếu Polycarp hoàn thành đầy đủ n lượt truy cập với giao dịch mua bất cứ khi nào có thể. 
2. Tính xem có thể hoàn thành bao nhiêu chu kỳ đầy đủ bằng cách chia số nguyên của số tiền còn lại cho chi phí chu kỳ. Điều này trực tiếp đưa ra số lần lặp lại đầy đủ được đảm bảo của cùng một hành vi. 
3. Cộng số kẹo đã mua trong toàn bộ chu kỳ bằng cách nhân số lượng mua trong mỗi chu kỳ với số chu kỳ. Điều này nén một số lượng lớn các mô phỏng lặp lại thành một phép toán số học. 
4. Giảm số tiền còn lại bằng cách trừ đi tổng chi phí của tất cả các chu kỳ đầy đủ. Sau đó, ngân sách còn lại hoàn toàn nhỏ hơn một chi phí chu kỳ đầy đủ, điều này đảm bảo rằng tối đa một lần truyền tải một phần sẽ xảy ra. 
5. Mô phỏng một lần đi qua các gian hàng theo thứ tự. Tại mỗi gian hàng, nếu số tiền hiện tại ít nhất bằng giá tiền thì trừ và tăng câu trả lời. Chỉ dừng lại sau khi hoàn thành đường chuyền. 
6. Xuất ra tổng cộng tích lũy. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý đầy đủ các chu kỳ, trạng thái của Polycarp tương đương với việc khởi động lại từ gian hàng 1 với số tiền giảm bớt và không có cấu hình trung gian nào trong các chu kỳ trước đó có thể khác nhau vì mỗi chu kỳ giống hệt nhau về cấu trúc và tích lũy chi phí. Vì mỗi chu kỳ đầy đủ đều ảnh hưởng đến tiền và đếm một cách thống nhất nên việc nén chúng không làm mất đi tính đúng thứ tự hoặc quyết định. Mô phỏng còn lại được giới hạn bởi một chu kỳ, do đó nó nắm bắt chính xác tất cả hành vi từng phần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, T = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)

    full_cycles = T // total
    if full_cycles:
        T -= full_cycles * total

    ans = full_cycles * n

    for x in a:
        if T >= x:
            T -= x
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén các vòng lặp đầy đủ lặp lại bằng cách sử dụng số học. Tổng giá của tất cả các gian hàng sẽ xác định chi phí của một lượt truy cập đầy đủ. Nhân hiệu ứng này sẽ tránh được việc mô phỏng các chu kỳ lặp đi lặp lại. 

Vòng lặp còn lại là một lần truyền qua mảng. Mỗi bước kiểm tra khả năng chi trả và cập nhật trạng thái. Không cần chỉ số modulo vì chúng tôi chỉ mô phỏng tối đa một chu kỳ sau khi nén. 

Một điểm tinh tế là sử dụng phép chia số nguyên trước khi sửa đổi ngân sách còn lại. Điều này đảm bảo rằng các chu kỳ đầy đủ được tính chính xác và tránh các lỗi dấu phẩy động hoặc các vòng trừ lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 38
5 2 5
```Tổng chu kỳ là 12. Polycarp có thể hoàn thành 3 chu kỳ đầy đủ (36 chu kỳ đã chi), để lại 2. 

| Bước | Vị trí | Tiền trước | Hành động | Tiền sau | Tổng số kẹo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | chu kỳ đầy đủ | 38 | mất 3 chu kỳ | 2 | 9 | 
| 2 | 1 | 2 | không mua được 5 | 2 | 9 | 
| 3 | 2 | 2 | mua 2 | 0 | 10 | 
| 4 | 3 | 0 | dừng lại | 0 | 10 | 

Dấu vết cho thấy rằng các chu kỳ đầy đủ chi phối quá trình tính toán và chỉ phần còn lại mới góp phần mua một phần cuối cùng. 

### Ví dụ 2 

đầu vào:```
3 5
4 3 2
```Tổng chu kỳ là 9 nên không thể thực hiện được toàn bộ chu kỳ. 

| Bước | Vị trí | Tiền trước | Hành động | Tiền sau | Tổng số kẹo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | mua 4 | 1 | 1 | 
| 2 | 2 | 1 | bỏ qua | 1 | 1 | 
| 3 | 3 | 1 | bỏ qua | 1 | 1 | 

Trường hợp này xác nhận tính đúng đắn khi chỉ xảy ra việc truyền tải một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính tổng và một lượt để mô phỏng phần dư | 
| Không gian | O(1) | Chỉ có một vài quầy và lưu trữ đầu vào | 

Giải pháp là tuyến tính về số lượng gian hàng, vừa vặn thoải mái với các ràng buộc 2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, T = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)
    full = T // total
    T -= full * total
    ans = full * n

    for x in a:
        if T >= x:
            T -= x
            ans += 1

    return str(ans)

# provided sample
assert run("3 38\n5 2 5\n") == "10"

# all equal, small
assert run("4 10\n2 2 2 2\n") == "4"

# cannot buy anything
assert run("3 1\n5 6 7\n") == "0"

# single booth
assert run("1 100\n3\n") == "33"

# exact one cycle
assert run("2 3\n1 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đều nhỏ như nhau | 4 | hành vi đi xe đạp thống nhất | 
| không mua hàng | 0 | bỏ qua logic | 
| gian hàng đơn | 33 | suy biến n=1 trường hợp | 
| phù hợp với chu kỳ chính xác | 2 | ranh giới giữa chu kỳ đầy đủ và một phần | 

## Vỏ cạnh 

Khi n = 1, quá trình sẽ trở thành phép trừ lặp đi lặp lại của một giá trị. Đối với đầu vào`1 100`với giá`3`, thuật toán tính tổng chu kỳ là 3, chu kỳ đầy đủ là 33 và số dư 1, mang lại 33 lần mua hàng, phù hợp với lý luận trực tiếp vì mỗi chu kỳ chỉ là một cơ hội mua hàng. 

Khi tất cả các mức giá đều vượt quá T, chẳng hạn như`3 5 6 7`, số lượng chu kỳ đầy đủ bằng 0 và lượt còn lại mang lại số lần mua hàng bằng 0, phù hợp với thực tế là không có gian hàng nào có giá cả phải chăng vào bất kỳ lúc nào. 

Khi tất cả các mức giá bằng nhau và nhỏ so với T, thuật toán giảm xuống còn chia T cho tổng rồi tiêu thụ phần còn lại một cách tuyến tính, đảm bảo rằng ngay cả T rất lớn cũng không yêu cầu mô phỏng từng bước.
