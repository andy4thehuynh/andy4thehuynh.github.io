+++
draft = true
author = "Andy Huynh"
date = "2015-06-25T17:26:11-07:00"
title = "Null Pattern for Strings"
+++


if params[:coupon_code].present?
  params[:coupon_code].upcase
else
  params[:coupon_code]
end


Nil guard by typecasting:

params[:coupon_code].to_s.upcase
