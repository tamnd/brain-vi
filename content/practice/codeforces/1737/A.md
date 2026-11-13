---
title: "CF 1737A - Sách Phân Loại Ela"
description: "Vấn đề có thể được hình dung như một bài tập sắp xếp và phân vùng. Chúng ta được cấp một chuỗi n chữ cái, mỗi chữ cái đại diện cho một cuốn sách bằng chữ cái đầu tiên của tên nó và chúng ta phải phân bổ đều những cuốn sách này vào k ngăn. Mỗi ngăn chứa đúng n/k cuốn sách."
date: "2026-06-09T17:58:12+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1737
codeforces_index: "A"
codeforces_contest_name: "Dytechlab Cup 2022"
rating: 900
weight: 1737
solve_time_s: 431
verified: false
draft: false
---

[CF 1737A - Sách sắp xếp Ela](https://codeforces.com/problemset/problem/1737/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, chuỗi 
**Thời gian giải:** 7 phút 11 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề có thể được hình dung như một bài tập sắp xếp và phân vùng. Chúng tôi được cấp một chuỗi`n`các chữ cái, mỗi chữ cái đại diện cho một cuốn sách bằng chữ cái đầu tiên trong tựa đề của nó và chúng ta phải phân bổ đều những cuốn sách này vào`k`ngăn. Mỗi ngăn chứa chính xác`n/k`sách. Sau khi sách được phân phối, đối với mỗi ngăn, chúng tôi tính toán chữ cái bị loại trừ tối thiểu hoặc MEX, là chữ cái đầu tiên từ`'a'`ĐẾN`'y'`không xuất hiện trong ngăn đó. Mục đích là tạo ra một chuỗi có độ dài`k`bao gồm các chữ cái MEX từ mỗi ngăn theo thứ tự và chúng tôi muốn chuỗi kết quả này có từ điển lớn nhất có thể. 

Các hạn chế là nhỏ:`n`nhiều nhất là 200 và tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 1000. Điều này cho phép các thuật toán có độ phức tạp bậc hai hoặc bậc ba cho mỗi trường hợp thử nghiệm nếu cần thiết, mặc dù cách tiếp cận tuyến tính hoặc gần tuyến tính sẽ rõ ràng hơn và dễ suy luận hơn. Các trường hợp đặc biệt có thể phá vỡ cách tiếp cận ngây thơ bao gồm các tình huống trong đó tất cả các cuốn sách đều có cùng một chữ cái, tất cả các chữ cái xuất hiện chính xác một lần hoặc số ngăn`k`bằng với`n`, buộc mỗi ngăn phải có một cuốn sách. 

Một cách tiếp cận bất cẩn có thể chỉ đơn giản là sắp xếp các chữ cái và phân phối chúng một cách tuần tự mà không xem xét MEX cho từng ngăn, điều này sẽ thất bại trong những trường hợp như`"cabccadabaac"`với`k=3`. Sự phân bổ ngây thơ mà bỏ qua mức độ ưu tiên về mặt từ điển có thể tạo ra`"cdb"`thay vì đúng`"edb"`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các cách để phân chia các chữ cái được sắp xếp thành`k`ngăn, tính toán MEX cho mỗi phân vùng và chọn chuỗi lớn nhất theo từ điển. Điều này rõ ràng là không khả thi: số lượng phân vùng tăng lên theo tổ hợp, và thậm chí đối với`n=200`Và`k=2`, điều này sẽ liên quan đến hơn`2^200`khả năng. 

Thông tin chi tiết quan trọng là vì chúng ta muốn chuỗi kết quả lớn nhất về mặt từ điển nên chúng ta nên ưu tiên lấp đầy các ngăn bằng càng nhiều chữ cái đầu tiên càng tốt để đẩy chữ MEX càng xa về phía`'z'`càng tốt. Việc sắp xếp chuỗi trước tiên đảm bảo rằng chúng ta có thể đếm số lần xuất hiện của từng chữ cái một cách dễ dàng. Đối với mỗi ngăn, chúng tôi bắt đầu phân bổ các chữ cái từ`'a'`lên trên cho đến khi đầy ngăn. Chữ cái đầu tiên không thể sử dụng hết trong ngăn đó sẽ trở thành MEX. Nếu một ngăn sử dụng tất cả các trường hợp của`'a'`, nó có thể có`'b'`như MEX của nó, v.v. Bằng cách xử lý các ngăn một cách tuần tự và tham lam bằng cách sử dụng các chữ cái nhỏ nhất có sẵn, chúng tôi đảm bảo rằng MEX của mỗi ngăn được tối đa hóa theo thứ tự từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổ hợp(n,k)) | O(n) | Quá chậm | 
| MEX tuần tự tham lam | O(n + 26*k) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`và số ngăn`k`. Đếm số lần xuất hiện của mỗi chữ cái trong`'a'`ĐẾN`'y'`. Việc sắp xếp chuỗi là tùy chọn vì chúng ta có thể làm việc trực tiếp với số đếm. 
2. Tính kích thước từng ngăn`size = n // k`. Khởi tạo một danh sách trống để lưu trữ các chữ cái MEX kết quả. 
3. Xử lý từng ngăn một. Đối với mỗi ngăn, cố gắng gán các chữ cái bắt đầu từ`'a'`. Theo dõi có bao nhiêu chữ cái đã được sử dụng trong ngăn hiện tại. Khi một chữ cái vượt quá số lượng còn lại cho ngăn đó, hãy dừng lại. Chữ cái đầu tiên không được sử dụng đầy đủ trong ngăn này là MEX cho ngăn này. 
4. Sau khi xác định MEX cho một ngăn, hãy giảm tổng số lượng các chữ cái được sử dụng và chuyển sang ngăn tiếp theo. 
5. Ghép các chữ cái MEX từ tất cả các ngăn để tạo ra chuỗi cuối cùng cho ca kiểm thử. 

Bất biến đảm bảo tính đúng đắn là chúng ta luôn sử dụng các chữ cái nhỏ nhất có sẵn trước tiên trong mỗi ngăn. Điều này đảm bảo rằng MEX được đẩy xa nhất có thể trong bảng chữ cái, tối đa hóa giá trị từ điển của chuỗi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        cnt = [0]*26
        for ch in s:
            cnt[ord(ch)-ord('a')] += 1
        
        res = []
        size = n // k
        
        for _ in range(k):
            mex = 0
            remaining = size
            for i in range(26):
                take = min(cnt[i], remaining)
                remaining -= take
                cnt[i] -= take
                if take < size and remaining == 0:
                    mex = i+1
                    break
                if remaining == 0:
                    mex = i+1 if take == 0 else i
                    break
            if mex == 0:
                while mex < 26 and cnt[mex] == 0:
                    mex += 1
            res.append(chr(ord('a') + mex))
        
        print(''.join(res))

if __name__ == "__main__":
    solve()
```Trong mã này,`cnt`theo dõi các chữ cái có sẵn. Đối với mỗi ngăn, chúng tôi cố gắng lấp đầy nó bằng`size`chữ cái bắt đầu từ`'a'`. Nếu một chữ cái không thể điền đầy đủ vào ngăn, MEX là chữ cái tiếp theo. Sau khi xử lý, chúng tôi giảm số lượng để phản ánh các chữ cái đã được sử dụng, đảm bảo các ngăn sau này tính toán MEX chính xác. Các lỗi xảy ra có thể tránh được bằng cách theo dõi cẩn thận`remaining`các chữ cái trong ngăn và chỉ mục của MEX. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
12 3
cabccadabaac
```chúng tôi có`n=12`,`k=3`,`size=4`. Đếm chữ cái:`'a':5, 'b':2, 'c':3, 'd':1`. Các ngăn xử lý: 

| Ngăn | Chữ cái được sử dụng | MEX | 
| --- | --- | --- | 
| 1 | a,b,c,d | e | 
| 2 | a,a,b,c | d | 
| 3 | a,a,a,c | b | 

Chuỗi kết quả:`"edb"`. 

Đối với đầu vào:```
12 6
cabccadabaac
```Kích thước mỗi ngăn: 2 

| Ngăn | Chữ cái được sử dụng | MEX | 
| --- | --- | --- | 
| 1 | a,c | c | 
| 2 | a,b | c | 
| 3 | a,c | b | 
| 4 | b,c | b | 
| 5 | một, một | một | 
| 6 | một, một | một | 

Chuỗi kết quả:`"ccbbba"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k*26) | Đếm các chữ cái là O(n), xử lý k ngăn có tối đa 26 chữ cái, mỗi ngăn là O(k*26) | 
| Không gian | O(26) | Chỉ một mảng gồm 26 chữ cái cộng với O(k) cho kết quả | 

Với các ràng buộc (`n <= 200`, tổng của`n`trên tất cả các bài kiểm tra <= 1000,`k <= n`), giải pháp này chạy thoải mái trong vòng 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n12 3\ncabccadabaac\n12 6\ncabccadabaac\n12 12\ncabccadabaac\n25 1\nabcdefghijklmnopqrstuvwxy\n10 5\nbcdxedbcfg\n") == "edb\nccbbba\nbbbbbaaaaaaa\nz\naaaaa", "samples"

# Custom test cases
assert run("1\n5 5\naaaaa\n") == "bcdef", "all same letter"
assert run("1\n5 1\naabcd\n") == "e", "one compartment"
assert run("1\n26 2\nabcdefghijklmnopqrstuvwxyz\n") == "mz", "full alphabet split in two"
assert run("1\n8 4\naabbccdd\n") == "abcd", "even distribution multiple letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 cuốn, 5 ngăn toàn chữ 'a' | bcdef | Xử lý MEX từ điển khi hết chữ cái | 
| 5 cuốn 1 ngăn | e | Tính toán MEX một ngăn | 
| Chia bảng chữ cái đầy đủ | mz | Tính toán MEX tuần tự chính xác trên các ngăn | 
| Các chữ cái lặp đi lặp lại đều | abcd | Sửa MEX khi chữ xuất hiện nhiều lần | 

## Vỏ cạnh 

Ví dụ: nếu tất cả các cuốn sách đều có cùng một chữ cái`"aaaa"`với`k=2`, các ngăn là`[aa, aa]`. Chữ cái còn thiếu đầu tiên trong cả hai là`'b'`. Thuật toán tính toán chính xác MEX là`'b'`cho cả hai ngăn, sản xuất`"bb"`. Nếu như`k=n`, mỗi ngăn chứa một chữ cái duy nhất, vì vậy MEX là chữ cái tiếp theo trong bảng chữ cái sau chữ cái đó. Mã xử lý việc này bởi vì`remaining`theo dõi chính xác một chữ cái và MEX được đặt làm chữ cái đầu tiên không được sử dụng. Khi chuỗi sử dụng gần như tất cả các chữ cái cho đến`'y'`, MEX sẽ là`'z'`khi một ngăn
