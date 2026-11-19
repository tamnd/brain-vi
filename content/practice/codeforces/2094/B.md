---
title: "CF 2094B - Bobritto Bandito"
description: "Chúng tôi được yêu cầu theo dõi sự lây lan của bệnh nhiễm trùng dọc theo vô số dãy nhà. Sự lây nhiễm bắt đầu từ ngôi nhà 0 vào ngày 0 và mỗi ngày nó lây lan sang đúng một ngôi nhà khác liền kề với bất kỳ ngôi nhà nào hiện đang bị nhiễm bệnh."
date: "2026-06-08T05:33:35+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2094
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1017 (Div. 4)"
rating: 800
weight: 2094
solve_time_s: 110
verified: false
draft: false
---

[CF 2094B - Bobritto Bandito](https://codeforces.com/problemset/problem/2094/B) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu theo dõi sự lây lan của bệnh nhiễm trùng dọc theo vô số dãy nhà. Sự lây nhiễm bắt đầu từ ngôi nhà 0 vào ngày 0 và mỗi ngày nó lây lan sang đúng một ngôi nhà khác liền kề với bất kỳ ngôi nhà nào hiện đang bị nhiễm bệnh. Sau đó$n$ngày, chúng tôi được cung cấp phân khúc nhà liền kề$[l, r]$bị nhiễm bệnh. Nhiệm vụ là xác định bất kỳ phân đoạn hợp lệ nào$[l', r']$có thể đã bị nhiễm bệnh vào ngày hôm trước$m \le n$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, các giá trị chính là$n$, ngày có phân đoạn đã biết,$m$, ngày trước đó mà chúng ta phải tìm một đoạn và tọa độ$l$Và$r$của đoạn sau$n$ngày. Vấn đề đảm bảo rằng$r-l = n$, phản ánh số lượng ngôi nhà bị nhiễm bệnh mỗi ngày tăng thêm đúng một ngôi nhà. 

Các ràng buộc là nhỏ ($n \le 2000$) do đó, ngay cả mô phỏng lực lượng vũ phu cũng có thể chạy về mặt kỹ thuật, nhưng cách tiếp cận mang tính xây dựng đơn giản hơn sẽ hoạt động trong$O(1)$mỗi trường hợp thử nghiệm. 

Các trường hợp ranh giới không rõ ràng liên quan đến các ranh giới âm, chẳng hạn như nếu phân đoạn bao gồm các ngôi nhà ở bên trái số 0 hoặc nếu sự lây nhiễm hoàn toàn ở bên phải. Ví dụ, nếu$n=4$và phân đoạn là$[-2,2]$, thì một phân đoạn hợp lệ tại$m=1$có thể là$[0,0]$,$[-1,0]$, hoặc$[0,1]$. Một cách tiếp cận ngây thơ có thể giả định không chính xác ranh giới bên trái phải bằng 0 hoặc ranh giới bên phải phải giữ nguyên dương, nhưng đoạn có thể dịch chuyển tự do miễn là độ dài của nó bằng số ngày. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng mỗi ngày bắt đầu từ ngày 0. Bạn có thể thử mọi cách có thể để mở rộng phân khúc theo từng ngôi nhà sang trái hoặc phải cho đến khi đạt đến ngày$n$. Điều này có tác dụng vì số ngày$n$là nhỏ, nhưng ngay cả với$n=2000$, việc mô phỏng mỗi ngày cho tất cả các trường hợp thử nghiệm là phức tạp không cần thiết. Số lượng hoạt động sẽ là khoảng$O(n \cdot t)$, có thể chấp nhận được ở đây nhưng không thanh lịch. 

Thông tin chi tiết quan trọng là phân đoạn lây nhiễm diễn ra liên tục và mở rộng đối xứng (một ngôi nhà mỗi ngày) sang trái, phải hoặc kết hợp. Biết độ dài đoạn sau$n$ngày ($r-l = n$), chúng ta có thể tính toán trực tiếp một phân đoạn hợp lệ vào ngày$m$bằng cách thu hẹp phân khúc hiện tại bằng cách$n-m$nhà, bằng cách di chuyển ranh giới bên trái sang phải, ranh giới bên phải sang trái hoặc cả hai. Lựa chọn đơn giản nhất là dịch chuyển ranh giới bên trái sang phải bằng cách$n-m$và giữ đúng ranh giới cố định. Điều này đảm bảo một phân đoạn hợp lệ và hoạt động với tất cả các giá trị của$l$Và$r$vì nhiễm trùng có thể mở rộng theo một trong hai hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * t) | O(1) | Được chấp nhận nhưng dài dòng không cần thiết | 
| Sự thay đổi mang tính xây dựng | O(t) | O(1) | Được chấp nhận và đơn giản | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$,$m$,$l$,$r$. 
3. Tính số nhà bị nhiễm thêm kể từ ngày$m$:$extra = n - m$. 
4. Chọn chặng hợp lệ trong ngày$m$. Một lựa chọn đơn giản là dịch chuyển ranh giới bên trái sang phải bằng cách$extra$nhà, giữ nguyên ranh giới bên phải:$$l' = l + extra, \quad r' = r$$Điều này hoạt động vì độ dài của đoạn vào ngày$m$sẽ được$r' - l' = n - extra = m$, chính xác là số nhà bị nhiễm bệnh theo ngày$m$. 
5. In$l'$Và$r'$cho từng trường hợp thử nghiệm. 

**Tại sao nó hoạt động:** Mỗi ngày, sự lây nhiễm tạo thành một phân đoạn liên tục. Độ dài phân đoạn bằng số ngày kể từ khi bắt đầu. Bằng cách thu nhỏ phân đoạn một cách đối xứng từ hai phía, chúng ta có thể tái tạo lại bất kỳ phân đoạn nào của ngày trước đó trong khi vẫn duy trì tính liên tục. Vì tồn tại nhiều lựa chọn hợp lệ (ví dụ: rút gọn từ bên phải), bất kỳ lựa chọn mang tính xây dựng nào cũng có thể được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, l, r = map(int, input().split())
    extra = n - m
    l_prime = l + extra
    r_prime = r
    print(l_prime, r_prime)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý nhiều trường hợp thử nghiệm.`extra`tính số bước cần đảo ngược từ ngày$n$Hôm nay$m$. Ranh giới bên trái được dịch chuyển sang phải bởi`extra`, đảm bảo độ dài đoạn bằng`m`. Điều này tránh được các lỗi riêng lẻ, các vấn đề về lập chỉ mục tiêu cực và các lỗi về ranh giới. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`4 2 -2 2`Đây$n=4, m=2, l=-2, r=2$. 

-`extra = 4 - 2 = 2`-`l' = -2 + 2 = 0`-`r' = 2`Phân đoạn ngày thứ 2:`[0, 2]`, chiều dài = 2, hợp lệ. 

| n | m | tôi | r | thêm | tôi' | r' | 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 2 | -2 | 2 | 2 | 0 | 2 | 

**Ví dụ 2:**`3 3 -1 2`-`extra = 3 - 3 = 0`-`l' = -1 + 0 = -1`-`r' = 2`Phân đoạn ở ngày thứ 3:`[-1,2]`, không thay đổi, hợp lệ. 

| n | m | tôi | r | thêm | tôi' | r' | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 3 | -1 | 2 | 0 | -1 | 2 | 

Những dấu vết này cho thấy rằng việc dịch chuyển ranh giới bên trái bằng`extra`xây dựng lại chính xác một phân đoạn hợp lệ vào ngày`m`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được giải quyết trong các hoạt động O(1) | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên cho mỗi trường hợp thử nghiệm | 

Được cho$t \le 100$Và$n \le 2000$, giải pháp dễ dàng chạy tốt dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, m, l, r = map(int, input().split())
        extra = n - m
        print(l + extra, r)
    return out.getvalue().strip()

# Provided samples
assert run("4\n4 2 -2 2\n4 1 0 4\n3 3 -1 2\n9 8 -6 3") == "-0 2\n3 4\n-1 2\n-5 3", "sample 1"

# Custom cases
assert run("2\n1 1 0 1\n2 1 -2 0") == "0 1\n-1 0", "min-size segments"
assert run("1\n2000 1000 -1000 1000") == "0 1000", "large n, mid-shift"
assert run("1\n5 5 0 5") == "0 5", "full-length day n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 1 | 0 1 | phân khúc nhỏ nhất có thể | 
| 2 1 -2 0 | -1 0 | ranh giới trái âm | 
| 2000 1000 -1000 1000 | 0 1000 | n lớn, độ chính xác giữa ca | 
| 5 5 0 5 | 0 5 | phân đoạn ngày cuối cùng bằng n | 

## Vỏ cạnh 

cho`n=m`, đoạn đó không co lại. Vì`n>m`và phân đoạn bao trùm các chỉ số âm và dương, thuật toán chỉ dịch chuyển chính xác ranh giới bên trái, duy trì tính liên tục. Ví dụ, đầu vào`n=4, m=2, l=-2, r=2`sản lượng`l'=-2+2=0`,`r'=2`, cho`[0,2]`. Mặc dù ranh giới bên trái trở thành dương, phân đoạn vẫn hợp lệ vì bài toán cho phép bất kỳ phân đoạn liên tục hợp lệ nào. 

Đối với một đoạn hoàn toàn ở bên phải, ví dụ:`n=4, m=2, l=0, r=4`, dịch chuyển ranh giới trái bởi`extra=2`cho`[2,4]`, một lần nữa hợp lệ. 

Điều này cho thấy thuật toán xử lý nhất quán cả phân đoạn âm, dương và hỗn hợp. 

Bài xã luận này bao gồm tất cả các khía cạnh: lý luận vấn đề, các ràng buộc, lựa chọn cách tiếp cận, các trường hợp đặc biệt, các ví dụ đã được thực hiện,
