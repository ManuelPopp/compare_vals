compare.neighbours <- function(x, mode = "queen", assign=NA){
require("raster")
if(is.numeric(mode)){
dims <- mode}else if(mode == "rook"){
dims <- 4}else if(mode == "queen"){
dims <- 8}else if(mode == "bishop"){
dims <- 16}
out <- x
pxls <- x@ncols*x@nrows
for(n in 1:pxls){
  val <- extract(x, n)
  nghbrs <- extract(x, adjacent(x, cells = n, directions = dims)[,2])
  if(isTRUE(all(val == nghbrs))){
    out[n] <- val
  }else if(!isTRUE(all(val == nghbrs))){
    out[n] <- assign
  }
return(out)
}

# way faster implementation to find class border pixels
require("raster")

extract_borders <- function(x, mode = 4){
  if(class(mode) == "character"){
    pos <- which(c("rook", "queen", "bishop") == mode)
    if(length(pos) < 1){
      warning("Invalid mode argument. Must be in 'rook', 'queen', or 4, 8.")
    }
    mode <- c(4, 8, 16)[pos]
  }
  resX <- raster::xres(x)
  resY <- raster::yres(x)
  if(mode == 4){
    shiftX <- raster::shift(x, dx = resX, dy = 0)
    sniftX <- raster::shift(x, dx = -resX, dy = 0)
    shiftY <- raster::shift(x, dx = 0, dy = resY)
    sniftY <- raster::shift(x, dx = 0, dy = -resY)
    ext1 <- raster::extent(raster::intersect(shiftX, sniftX))
    ext2 <- raster::extent(raster::intersect(shiftY, sniftY))
    ext <- raster::extent(raster::intersect(ext1, ext2))
    e <- raster::extent(raster::intersect(x, ext))
    out <- raster::crop(x, e)
    out[] <- FALSE
    out[raster::crop(x, e) - raster::crop(shiftX, e) != 0 |
          raster::crop(x, e) - raster::crop(shiftY, e) != 0 |
          raster::crop(x, e) - raster::crop(sniftX, e) != 0 |
          raster::crop(x, e) - raster::crop(sniftY, e) != 0 |
          raster::crop(x, e) - raster::crop(shiftX, e) != 0 |
          raster::crop(x, e) - raster::crop(shiftY, e) != 0] <- TRUE
  }else{
    shiftXY <- raster::shift(x, dx = resX, dy = resY)
    sniftXY <- raster::shift(x, dx = -resX, dy = -resY)
    shiftYX <- raster::shift(x, dx = resX, dy = -resY)
    sniftYX <- raster::shift(x, dx = -resX, dy = resY)
    ext1 <- raster::extent(raster::intersect(shiftXY, sniftXY))
    ext2 <- raster::extent(raster::intersect(shiftYX, sniftYX))
    ext <- raster::extent(raster::intersect(ext1, ext2))
    e <- raster::extent(raster::intersect(x, ext))
    out <- raster::crop(x, e)
    out[] <- FALSE
    if(mode == 1){
      out[raster::crop(x, e) - raster::crop(shiftXY, e) != 0 &
            raster::crop(x, e) - raster::crop(sniftYX, e) != 0 |
            raster::crop(x, e) - raster::crop(shiftYX, e) != 0 &
            raster::crop(x, e) - raster::crop(sniftXY, e) != 0] <- TRUE
    }else if(mode == 8){
      out[raster::crop(x, e) - raster::crop(shiftXY, e) != 0 |
            raster::crop(x, e) - raster::crop(sniftXY, e) != 0 |
            raster::crop(x, e) - raster::crop(shiftYX, e) != 0 |
            raster::crop(x, e) - raster::crop(sniftYX, e) != 0] <- TRUE
    }
  }
  return(out)
}
